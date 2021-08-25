<template>
  <div id="recommend">
    <div class="recommend-item" v-for="i in recommends" :key="i.id">
      <a :href="i.link">
        <img :src="i.image" alt="" />
        <div class="text">{{ i.title }}</div>
      </a>
    </div>
  </div>
</template>

<script>
export default {
  name: "Recommend",
  props: {
    recommends: {
      type: Array,
      default() {
        return [];
      },
    },
  },
};
</script>

<style>
#recommend {
  display: flex;

  text-align: center;
  font-size: 12px;
}
.recommend-item {
  flex: 1;
  border-bottom: 10px solid #eee;
  padding: 10px 0 20px;
}
.recommend-item img {
  width: 70px;
  margin-bottom: 10px;
}
</style>