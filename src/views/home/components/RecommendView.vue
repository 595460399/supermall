<template>
  <div class="recommend">
    <div v-for="item in recommends" :key="item.link" class="recommend-item">
      <a :href="item.link"></a>
      <img :src="item.image" alt="" />
      <div>{{ item.title }}</div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'RecommendView',
  props: {
    recommends: {
      type: Array,
      default: () => {}
    }
  }
}
</script>

<style lang="less" scoped>
.recommend {
  display: flex;
  text-align: center;
  font-size: 12px;
  padding: 15px 0 20px;
  border-bottom: 10px solid #eee;
}
.recommend-item {
  flex: 1;
  img {
    width: 80%;
    margin-bottom: 10px;
  }
}
</style>
