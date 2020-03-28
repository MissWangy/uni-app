<template>
  <view class="pages-home">
    <cell title="拖拽排序" is-link url="/pages/template/dragsort" />
    <cell title="iframe" is-link url="/pages/template/iframe/iframe-fu" />
    <cell title="upload" is-link url="/pages/template/upload/upload" />
  </view>
</template>

<script>
import Cell from "../../components/vant/cell/index";
import CellGroup from "../../components/vant/cell-group/index";
export default {
  name: "pages-home",
  components: {
    Cell,
    CellGroup
  },
  onLoad() {},
  onUnload() {},
  methods: {}
};
</script>

<style lang="less">
.pages-home {
}
</style>
