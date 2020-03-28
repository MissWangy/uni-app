<template>
  <view class="pages-template-dragsort">
    <dragSort
      :list="list"
      :props="props"
      @click="onDragSortHandle"
      @change="onDragSortChange"
    ></dragSort>
  </view>
</template>

<script>
import dragSort from "@/components/drag-sort/index";
export default {
  name: "pages-template-dragsort",
  components: {
    dragSort
  },
  data() {
    return {
      props: {
        label: "label"
      },
      list: [
        {
          label: "标题1"
        },
        {
          label: "标题2"
        },
        {
          label: "标题3"
        },
        {
          label: "标题4"
        },
        {
          label: "标题5"
        }
      ]
    };
  },
  onLoad() {},
  onUnload() {},
  methods: {
    onDragSortChange(e) {
      console.log(e);
      // frontData 插到谁后面
      // data 操作的数据
    },
    onDragSortHandle(data) {
      console.log(data.label);
    }
  }
};
</script>

<style lang="less">
.pages-template-dragsort {
}
</style>
