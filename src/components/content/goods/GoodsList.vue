<template>
  <div class="goods-list">
    <div class="goods" v-for="item in goods" :key="item.id">
      <goods-list-item :goodsItem="item">{{ item }}</goods-list-item>
    </div>
  </div>
</template>

<script>
import GoodsListItem from "./GoodsListItem.vue";
export default {
  name: "GoodsList",
  components: { GoodsListItem },
  props: {
    goods: {
      type: Array,
      default() {
        return [];
      },
    },
  },
};
</script>

<style scoped>
</style>