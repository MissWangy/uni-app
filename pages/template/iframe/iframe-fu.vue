<template>
  <view class="pages-template-iframe-iframe-fu">
    <!-- <view>从子页面传递的数据：{{childData}}</view> -->
    <button class="sendMessage-btn" @tap="sendMessageToSon">
      向iframe子组件发送消息
    </button>

    <view class="message-text">{{
      `接收从iframe子组件传递过来的数据${
        childData && childData.data ? childData.data : ""
      }`
    }}</view>

    <view class="pages-template-iframe-iframe-fu-body">
      <view class="pages-template-iframe-iframe-fu-body__border">
        <view class="pages-template-iframe-iframe-fu-body__border-content">
          <iframe
            ref="iframe"
            id="Iframe"
            :src="src"
            width="100%"
            height="100%"
            frameborder="0"
            scrolling="auto"
            sandbox="allow-same-origin allow-top-navigation allow-forms allow-scripts"
          ></iframe>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
let info = {
  src: "https://img.yzcdn.cn/vant/leaf.jpg",
  title: "哎呀",
  background: "#f00",
  fontSize: "30rpx"
};
export default {
  name: "pages-template-iframe-iframe-fu",
  data() {
    return {
      childData: null,
      src:
        "http://localhost:8080/static-resource/vant-uni/#/pages/template/iframe/iframe-zi"
    };
  },
  onLoad() {
    window.addEventListener("message", this.receiveMessage, false);
  },
  onUnload() {
    window.removeEventListener("message", this.receiveMessage);
  },
  methods: {
    //向子组件发送消息
    sendMessageToSon() {
      // let iframeWin = window.frames[0]
      //  iframeWin.postMessage(info, 'http://localhost:8080')
      const childFrameObj = document.getElementById("Iframe");
      childFrameObj.contentWindow.postMessage(info, "http://localhost:8080");
    },

    //接收子组件发送过来的信息
    receiveMessage(e) {
      this.childData = e.data;
    }
  }
};
</script>

<style lang="less">
.pages-template-iframe-iframe-fu {
  width: 100%;
  height: 1200rpx;

  .message-text {
    width: 100%;
    padding: 30rpx;
    font-size: 30rpx;
    text-align: center;
  }

  .sendMessage-btn {
    // margin-bottom: 50rpx;
  }

  &-body {
    width: 100%;
    height: 100%;
    background: url(https://gw.alipayobjects.com/zos/rmsportal/FymmugeGFRUmRVIjPyZH.png)
      no-repeat 0 0;
    background-size: 100% 100%;
    overflow: hidden;
    border-radius: 110rpx;
    &__border {
      margin: 0 auto;
      width: 98%;
      padding: 80rpx 20rpx 100rpx;
      box-sizing: border-box;
      height: 100%;
      border-radius: 100rpx;
      border: 12rpx solid #dadada;
      box-shadow: 0 20rpx 64rpx rgba(0, 0, 0, 0.1);
      box-sizing: border-box;
      overflow: hidden;
      &-content {
        width: 100%;
        height: 100%;
        box-sizing: border-box;
        border: 1rpx solid #000;
        padding-top: 100rpx;
        overflow: auto;
      }

      // &::after {
      //   content: "";
      //   position: absolute;
      //   bottom: 70rpx;
      //   left: 50%;
      //   margin-left: -24rpx;
      //   width: 60rpx;
      //   height: 60rpx;
      //   border-radius: 100%;
      //   border: 2rpx solid #dadada;
      // }
    }
  }
}
</style>
