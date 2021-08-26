<template>
  <div class="tab-control">
    <div
      class="tab-control-item"
      v-for="(item, index) in title"
      :key="item.id"
      @click="itemClick(index)"
    >
      <span :class="{ active: index === isActive }">{{ item }}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "TabControl",
  props: {
    title: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  data() {
    return {
      isActive: 0,
    };
  },
  methods: {
    itemClick(index) {
      this.isActive = index;
      this.$emit("itemClick", index);
    },
  },
};
</script>

<style scoped>
.tab-control {
  display: flex;
  position: sticky;
  top: 44px;
  height: 40px;
  line-height: 40px;
  background-color: #fff;
  text-align: center;
}
.tab-control-item {
  flex: 1;
}
.active {
  padding: 5px;
  color: var(--color-high-text);
  border-bottom: 2px solid var(--color-high-text);
}
</style>