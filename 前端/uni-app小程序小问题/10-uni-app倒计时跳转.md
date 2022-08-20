# uni-app倒计时小功能开发

目的：点击结算按钮，如果用户没有登录，那么延时3秒跳转到登录界面

代码中自有黄金屋，直接端上来罢：

```vue
<template>
  <view class="my-settle-container">
    <!-- 全选 -->
    <label class="radio">
      <radio @click="changeAllState" color="#C00000" :checked="isAllChecked" /><text>全选</text>
    </label>
    <!-- 合计 -->
    <view class="amount-box">
      合计: 
      <text class="amount">￥{{checkedGoodsAmount}}</text>
    </view>
    <!-- 结算，按钮 -->
    <view @click="settlement" class="btn-settle">
      结算({{checkedCount}})
    </view>
  </view>
</template>

<script>
  import {mapState,mapGetters,mapMutations} from 'vuex';
  export default {
    name:"my-settle",
    data() {
      return {
        // 倒计时秒数
        second: 3,
        // 定时器
        timer: null
      };
    },
    computed: {
      ...mapGetters('m_cart', ['checkedCount', 'total','checkedGoodsAmount']),
      ...mapGetters('m_user', ['addstr']),
      ...mapState('m_user', ['token']),
      isAllChecked() {
        return this.checkedCount === this.total ? true: false;
      }
    },
    methods: {
      ...mapMutations('m_cart', ['updateAllGoodsState']),
      // 全选
      changeAllState() {
        // console.log(this.isAllChecked)
        // this.isAllChecked要取反，应为点击就会输出
        this.updateAllGoodsState(!this.isAllChecked);
      },
      // 用户点击结算按钮
      settlement() {
        // 1.判断用户是否选择商品，商品数
        if(!this.checkedCount) return uni.$showMsg('请选择要结算的商品');
        
        // 2.判断用户是否选择收获地址
        if(!this.addstr) return uni.$showMsg('请选择收获地址');
        
        // 3.判断用户是否登录，判断token，如果用户没有登录，3秒后跳转到我的，登录界面
        if(!this.token) {
          // 调用跳转方法
          this.delayNavigate();
        };
        
      },
      // 展示倒计时提示消息
      showTips(n) {
        uni.showToast({
          icon:'none',
          title: '请登陆后再结算!' + n + '秒之后跳转到登录页。',
          mask: true, // 防止点击穿透，就是展示提示时，点击不了
          duration: 1500
        })
      },
      // 延时导航到我的页面，让用户去登录
      delayNavigate() {
        // 每次调用这个方法时，就重置一下second，防止second会变成0
        this.second = 3;
        this.showTips(this.second);
        // 开启一个定时器，每过1s就减少一下second，并再次调用showTips
        this.timer = setInterval(() => {
          this.second--;
          if(this.second <= 0) {
            clearInterval(this.timer);
            uni.switchTab({
              url: '/pages/my/my'
            })
            return; // 直接return，就不会走下面的showTips了
          };
          this.showTips(this.second); 
        }, 1000)
      }
    }
  }
</script>

```



