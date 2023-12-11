# Redis实现缓存

我们都简单学习过计算机组成原理中介绍的，在一台主机上，CPU访问内存的速度，要比访问缓存的速度要快的多的多。

所以呢，我们可以将项目中，一些多用户，操作频繁的数据，存储在redis上，减少数据库的操作，从而增加系统的**效率**(此效率包含多种含义)。

>注意：
>
>以下操作，都建立在`Spring Data Redis `上

## 增删改查的缓存

后端代码大部分都是在增删改查上，所以我们缓存也是从这上面开始入手。



**增：**

增加的操作，我们都建议是直接根据生成的id，去清除一下缓存中存储的数据，目的是为了保持数据一致性，增加后，我们要及时清理缓存，防止缓存任然存储着老数据。

基本操作方式：

```java
/**
 * 新增菜品
 * @param dishDTO
 * @return
 */
@PostMapping
@ApiOperation("新增菜品")
public Result save(@RequestBody DishDTO dishDTO) {
    log.info("新增菜品：{}", dishDTO);

    dishService.saveWithFlavor(dishDTO);

    // 新增操作，要清除redis
    // 找到指定key进行删除操作
    String key = "dish_" + dishDTO.getCategoryId(); // 注意这里的key的规则是我们自己指定
    redisTemplate.delete(key);
    
    // 下面的这个函数是封装代码
    // cleanCache(key);


    return Result.success();
}
```





**删：**

删除一般包含批量删和单独删除，往往后端在编写代码时都是选择二合一的，将两个方式都写在同一个service，mapper上，所以这里建议，删除时直接清空所有缓存

基本代码：

```java
    /**
     * 菜品批量删除
     * @param ids
     * @return
     */
    @DeleteMapping
    @ApiOperation("菜品批量删除")
    public Result delete(@RequestParam List<Long> ids) {
        log.info("菜品批量删除：{}", ids);
        dishService.deleteBatch(ids);

        // 由于删除操作设计多个表，单一清理redis，还需要再次查询数据库
        // 这里为了效率考量，直接选择清除所有菜品相关的redis数据

        // 第一步操作获取所有dish_开头的key
        Set keys = redisTemplate.keys("dish_*");
        // 第二步直接删除查询到的key
        redisTemplate.delete(keys);
        
        
        // cleanCache("dish_*");

        return Result.success();
    }
```





**改：**

改的操作，也是一样有时候，我们往往修改涉及多张表操作，为了降低缓存操作复杂性，我们这里任然建议直接**清楚所有缓存**

基本代码：

```java
@PutMapping
@ApiOperation("修改菜品信息")
public Result update(@RequestBody DishDTO dishDTO) {
    log.info("修改菜品：{}", dishDTO);
    dishService.updateWithFlavor(dishDTO);

    // 注意：这里修改菜品信息时，有一个特殊情况，就是修改菜品分类的时候
    // 会涉及两个分类，自然涉及到redis中的两个数据
    // 这里为了不复杂话代码，选择直接删除就好了
    // 第一步操作获取所有dish_开头的key
    Set keys = redisTemplate.keys("dish_*");
    // 第二步直接删除查询到的key
    redisTemplate.delete(keys);

    // cleanCache("dish_*");

    return Result.success();
}
```



**查：**

查的操作，相对来说稍微复杂一点，基本的判断逻辑如下

第一步：查询缓存，缓存如果存在，直接返回缓存数据

第二步：如果缓存不存在，查询数据库，返回数据库内容，并将内容更新进缓存中



基本代码：

```java
/**
 * 根据分类id查询菜品
 *
 * @param categoryId
 * @return
 */
@GetMapping("/list")
@ApiOperation("根据分类id查询菜品")
public Result<List<DishVO>> list(Long categoryId) {

    // 构造redis中的key  构造规则：dish_分类id
    String key = "dish_" + categoryId;
    // 查询redis中是否存在菜品数据
    // 注意，在Java操作redis中，我们放到redis中什么类型数据，取的时候就用什么类型数据
    List<DishVO> list = (List<DishVO>) redisTemplate.opsForValue().get(key);
    if(list != null && list.size() > 0) {
        // 如果存在，直接返回，无需查询数据库
        return Result.success(list);
    }
    // 如果不存在，则查询数据库，并且更新redis，将查询出来的数据放入redis
    Dish dish = new Dish();
    dish.setCategoryId(categoryId);
    dish.setStatus(StatusConstant.ENABLE);//查询起售中的菜品
    list = dishService.listWithFlavor(dish);
    // 将查询到的数据放到redis中
    redisTemplate.opsForValue().set(key, list);

    return Result.success(list);
}
```





