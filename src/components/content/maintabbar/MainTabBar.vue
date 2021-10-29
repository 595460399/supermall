<template>
  <div>
    <tab-bar>
      <tab-bar-item path="/home" activeColor="pink">
        <img
          slot="item-icon"
          src="../../../assets/img/tabbar/home.svg"
          alt=""
        />
        <img
          slot="item-icon-active"
          src="../../../assets/img/tabbar/home_active.svg"
          alt=""
        />
        <div slot="item-text">Home</div>
      </tab-bar-item>
      <tab-bar-item path="/category" activeColor="pink">
        <img
          slot="item-icon"
          src="../../../assets/img/tabbar/category.svg"
          alt=""
        />
        <img
          slot="item-icon-active"
          src="../../../assets/img/tabbar/category_active.svg"
          alt=""
        />
        <div slot="item-text">Category</div>
      </tab-bar-item>
      <tab-bar-item path="/cart" activeColor="pink">
        <img
          slot="item-icon"
          src="../../../assets/img/tabbar/shopcart.svg"
          alt=""
        />
        <img
          slot="item-icon-active"
          src="../../../assets/img/tabbar/shopcart_active.svg"
          alt=""
        />
        <div slot="item-text">Cart</div>
      </tab-bar-item>
      <tab-bar-item path="/profile" activeColor="pink">
        <img
          slot="item-icon"
          src="../../../assets/img/tabbar/profile.svg"
          alt=""
        />
        <img
          slot="item-icon-active"
          src="../../../assets/img/tabbar/profile_active.svg"
          alt=""
        />
        <div slot="item-text">Profile</div>
      </tab-bar-item>
    </tab-bar>
  </div>
</template>

<script>
import TabBar from '../../common/tabbar/TabBar.vue'

import TabBarItem from '../../common/tabbar/TabBarItem.vue'

export default {
  name: 'MainTabBar',
  components: {
    TabBar,
    TabBarItem
  }
}
</script>

<style lang="less" scoped></style>
