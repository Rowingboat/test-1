<template>
  <view class="index">
    <!-- auto-upload 为true时直接提示报错 -->
    <nut-uploader :url="uploadUrl" @change="change" :auto-upload="false" v-model:file-list="defaultFileList" maximum="3" multiple>
    </nut-uploader>
  </view>
</template>

<script>
import { reactive, toRefs } from 'vue';
import Taro from "@tarojs/taro"
export default {
  name: 'IndexG',
  components: {

  },
  setup() {
    const state = reactive({
      msg: '欢迎使用 NutUI3.0 开发小程序',
      msg2: '你成功了～',
      type: 'text',
      show: false,
      uploadUrl: 'https://xxx.com/upload/image',
      cover: false,
      defaultFileList: []
    });

    const change = ({ fileList }) => {
      Taro.uploadFile({
        url: state.uploadUrl,
        name: 'file',
        fileType: "image",// 添加该属性 没有报错
        fileName: "file",
        filePath: fileList[0].url,
        success: ({ data }) => {
          const urlData = JSON.parse(data)
          console.log(urlData);
        },
        fail: (error) => {
          console.log(error);
        }
      })
    }

    const handleClick = (type, msg, cover = false) => {
      state.show = true;
      state.msg2 = msg;
      state.type = type;
      state.cover = cover;
    };

    return {
      ...toRefs(state),
      change,
      handleClick
    }
  }
}
</script>

<style lang="scss">
.index {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
</style>
