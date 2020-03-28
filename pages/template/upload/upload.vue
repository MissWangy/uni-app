<template>
  <view class="pages-template-upload">
    <view class="upload-img">
      <van-uploader
        name="3"
        :file-list="fileList"
        multiple
        :max-count="maxCount"
        @after-read="afterRead"
        @delete="deletes"
        @click-preview="clickPreview"
      />
    </view>
  </view>
</template>

<script>
import VanUploader from "@/components/vant/uploader/index";
export default {
  name: "pages-template-upload",
  components: {
    VanUploader
  },
  data() {
    return {
      fileList: [],
      maxCount: 6,
      qulity: 0.7 //压缩系数
    };
  },
  onLoad() {},
  onUnload() {},
  methods: {
    /**
     * 1.获取文件信息
     * 2.将img绘制到canvas上，canvas.toDataURL压缩图片
     *  cavasObj.drawImage(img, x, y, width, height) 其中参数img 为要处理的源图片，x为canvas生成图片的起始x位置，y为图片的起始y位置，width为生成图片最终的宽度，heigth为图片最终高度。当width(height)小于源图片的宽度（高度）时，图片就被压缩了。
     * 3.new Blob将压缩后的Base64转为Blob格式 压缩后的canvas上的图片转化为blob，上传到服务器 使用formData对象生成表单，这里使用axios提交数据
     *
     */
    afterRead(event) {
      console.log(event);
      //获取选择上传的图片
      const { file, name } = event;
      const fileList = this.fileList;
      file.forEach(element => {
        const tempFile = { ...element };
        //压缩图片
        // fileList.push(tempFile)
        // this.fileList = fileList
        this.compressImg(tempFile);
      });
    },

    //压缩图片
    compressImg(file) {
      uni.getImageInfo({
        src: file.path,
        success: async result => {
          // console.log("rs", result)
          const { width, height } = result;
          const ratio = width / 1280 > 1 ? width / 1280 : 1;

          let img = new Image();
          img.src = result.path;
          // 默认图片质量为0.7  quality值越小，所绘制出的图像越模糊
          // 大于1M,压缩系数为。7
          const qulity =
            (file.size || 0) > 1024 * 1000 ? this.qulity : this.qulity;

          //生成canvas
          let canvas = document.createElement("canvas");
          let ctx = canvas.getContext("2d");

          //给canvas添加宽高
          canvas.width = parseInt(width / ratio);
          canvas.height = parseInt(height / ratio);

          //将图片画到canvas上面，使用canvas压缩
          ctx.drawImage(img, 0, 0, canvas.width, canvas.height);

          //生成文件，转化为blob
          // toDataURL参数为PNG时不支持传图片质量，所以需要写死image/jpeg或image/webp，具体可以参考toDataURL的api
          let dataurl = canvas.toDataURL("image/jpeg", qulity); // 图片格式jpeg或webp可以选0-1质量区间
          const blob = this.base64ToBlob(dataurl);

          file.blob = blob;
          file.size = blob.size;
          await this.uploadFile(file);
        }
      });
    },

    /**
     * 将以base64的图片url数据转换为Blob
     * @param urlData
     * 用url方式表示的base64图片数据
     */
    base64ToBlob(urlData) {
      let arr = urlData.split(",");
      let mime = arr[0].match(/:(.*?);/)[1],
        bstr = atob(arr[1]),
        n = bstr.length,
        u8arr = new Uint8Array(n);

      while (n--) {
        u8arr[n] = bstr.charCodeAt(n);
      }

      return new Blob([u8arr], {
        type: mime
      });
    },

    //上传图片
    uploadFile(file) {
      const { blob } = file;
      var formData = new FormData(); // FormData 对象
      /**
       * 第一个参数：文件类型
       * 第二个参数：转化后的blob参数
       * 第三个参数：filename是有浏览器兼容性问题的，如果不传就是filename=blob，后端校验文件名可能过不去
       */
      formData.append(
        "file",
        blob,
        "file_" + file.name || Date.parse(new Date()) + ".jpg"
      ); // 文件对象
      //判断转化后的文件大小是否大于1M
      if (blob.size > 1024 * 1000) {
        uni.showToast({
          title: "上传的图片不能大于1M",
          icon: "none"
        });
        return;
      }

      try {
        //上传图片 发送formData数据
        uni.showLoading({
          title: "上传中...",
          mask: true
        });

        setTimeout(() => {
          console.log(file);
          if (this.fileList.length >= this.maxCount) {
            uni.showToast({
              title: `最多上传${this.maxCount}张`,
              icon: "none"
            });
            return;
          }
          this.fileList = this.fileList.concat(file);
        }, 2000);
      } catch (e) {
        console.log(e);
      } finally {
        uni.hideLoading();
      }
    },

    //删除图片
    deletes(event) {
      const { index, name } = event;
      const fileList = this.fileList;
      fileList.splice(index, 1);
      this.fileList = fileList;
    },
    clickPreview() {}
  }
};
</script>

<style lang="less">
.pages-template-upload {
  .upload-img {
    padding: 30rpx;
  }
}
</style>
