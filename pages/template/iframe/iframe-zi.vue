<template>
  <view class="pages-template-iframe-iframe-zi">
    <view class="iframe-content">
      <view class="iframe-content_image">
        <image
          style="width:100%;height:100%"
          :src="
            parentData && parentData.src
              ? parentData.src
              : 'https://img.yzcdn.cn/vant/tree.jpg'
          "
        />
      </view>

      <view class="iframe-content_title">{{
        parentData && parentData.title
          ? parentData.title
          : "我是iframe组件通讯子页面"
      }}</view>

      <button class="sendMessage-btn" @tap="sendMessageToParent">
        向父组件iframe发送消息
      </button>
    </view>
  </view>
</template>

<script>
export default {
  name: "pages-template-iframe-iframe-zi",
  data() {
    return {
      color: "",
      fontSize: "",
      parentData: null
    };
  },
  onLoad() {
    window.addEventListener("message", this.receiveMessage, false);
  },
  onUnload() {
    window.removeEventListener("message", this.receiveMessage);
  },
  methods: {
    receiveMessage(e) {
      this.parentData = e.data;
    },

    //向父组件发送信息
    sendMessageToParent() {
      window.parent.postMessage(
        { data: "hello word" },
        "http://localhost:8080"
      );
    }
  }
};
</script>

<style lang="less">
.pages-template-iframe-iframe-zi {
  .sendMessage-btn {
    margin-bottom: 50rpx;
  }

  .iframe-content {
    width: 100%;
    height: 200rpx;
    margin: 30rpx auto;

    &_image {
      width: 100%;
      height: 400rpx;
    }

    &_title {
      width: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      margin-top: 50rpx;
      padding: 50rpx;
      box-sizing: border-box;
    }
  }
}
</style>
