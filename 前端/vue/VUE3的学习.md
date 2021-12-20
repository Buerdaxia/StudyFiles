# VUE3的学习

## toRef，和toRefs

目的：就是想把对象里面的属性拆散了return出去，不想写person.name啥的，就像单独要某一个属性

功能就是：将普通数据转换为响应式数据

toRef(对象,'对象中某个属性');

直接上代码

```js
    setup() {
      //数据
      let person = reactive({
        name: '张三',
        age: 18,
        job: {
          j1: {
            salary: 20
          }
        }
      })
      //这样name1就是响应式的数据并且是person的name属性
      const name1 = toRef(person, 'name');
      
      return {
        person,
        //模板里直接{{name}}但是没响应式了
		name: person.name,//这样就会丢失了响应式
        age: person.age,
        name1//由于上面的toRef修饰过所以是响应式的
      }
    }
```

