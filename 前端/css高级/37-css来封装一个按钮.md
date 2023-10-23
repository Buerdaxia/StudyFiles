# css来封装一个按钮

两个核心关键点：

1. 使用a标签
2. 配合使用a标签相关的伪类





## 一个按钮封装案例



```scss
// button组件样式（组件时全局都有，公共的模块）
.btn {
  /* :link伪类指链接未访问时 */
  &, // &代表.btn
  &:link,
  &:visited {
    position: relative;
    display: inline-block;
    text-transform: uppercase;
    text-decoration: none;
    padding: 1.5rem 4rem;
    border-radius: 10rem;
    transition: all 0.2s;
    font-size: $default-font-size;

    // 修改button标签的外边框
    border: none;
    cursor: pointer;
  }

  &:hover {
    transform: translateY(-0.3rem);
    box-shadow: 0 1rem 2rem rgba($color-black, 0.2);
    /* 注意这个，这个时hover后伪元素的变化，这个很重要！ */
    &::after {
      transform: scaleX(1.4) scaleY(1.6);
      opacity: 0;
    }
  }
  /* active是激活时 */
  &:active,
  &:focus {
    outline: none;
    transform: translateY(-0.1rem);
    box-shadow: 0 0.5rem 1rem rgba($color-black, 0.2);
  }

  &--white {
    background-color: $color-white;
    color: $color-gray-dark;
      /* 设置通用伪元素的bgc */
    &::after {
      background-color: $color-white;
    }
  }

  &--green {
    background-color: $color-primary-light;
    color: $color-gray-dark;
      /* 设置通用伪元素的bgc */
    &::after {
      background-color: $color-primary-light;
    }
  }

  &::after {
    content: '';
    /* 一般和父元素一致 */
    display: inline-block;
    height: 100%;
    width: 100%;
    border-radius: 10rem;
    /* 使用绝对定位把它放在按钮后面 */
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1;
    transition: all 0.4s;
  }

  &--animated {
    animation: moveInBottom 0.5s ease-out 0.75s;
    animation-fill-mode: backwards;
  }
}

.btn-text {
  &:link,
  &:visited {
    font-size: $default-font-size;
    color: $color-primary;
    display: inline-block;
    text-decoration: none;
    border-bottom: 1px solid $color-primary;
    padding: 3px;
    transition: all .2s;
  }

  &:hover {
    background-color: $color-primary;
    color: white;
    box-shadow: 0 1rem 2rem rgba($color-black, .15);
    transform: translateY(-2px);
  }

  &:active {
    box-shadow: 0 .5rem 1rem rgba($color-black, .15);
    transform: translateY(0);
  }
}
```

