<template>
  <div class="main">
    <!-- 云盘 -->
    <div class="yun-file bra">
      <div class="yun-file-title flex">
        <text class="f28 fw5 c0">云盘</text>
        <text class="f24 c153 fw4 pl20 pt10 pb10" @click="yunFileMoreEvent">全部</text>
      </div>
      <div class="yun-file-content">
        <div class="pb35 bb">
          <text class="f24 fw4 c102">最近使用的文档</text>
          <div class="flex mt36">
            <scroller class="yun-scroller" show-scrollbar='false' scroll-direction="horizontal">
              <div class="flex-ac file-name" v-for="(item, index) in yunFileArr" :key='index' @click='yunFileUserEvent'>
                <bui-image src="/image/test.jpg" width="52px" height="52px"></bui-image>
                <div class="flex-jc">
                  <text class="f24 c51 fw4 mt17 lines2">123312123321321</text>
                </div>
              </div>
            </scroller>
          </div>
        </div>
        <div class="pb35 bb mt31">
          <text class="f24 fw4 c102">我分享的文档</text>
          <div class="flex mt36">
            <scroller class="yun-scroller" show-scrollbar='false' scroll-direction="horizontal">
              <div class="flex-ac file-name" v-for="(item, index) in yunFileArr" :key='index' @click='yunFileUserEvent'>
                <bui-image src="/image/test.jpg" width="52px" height="52px"></bui-image>
                <div class="flex-jc">
                  <text class="f24 c51 fw4 mt17 lines2"
                    style="text-align-last: center;justify-content:flex-end">哈哈哈哈哈哈哈哈</text>
                </div>
              </div>
            </scroller>
          </div>
        </div>
        <div class="pb35 mt31">
          <text class="f24 fw4 c102">分享给我的文档</text>
          <div class="flex mt36">
            <scroller class="yun-scroller" show-scrollbar='false' scroll-direction="horizontal">
              <div class="flex-ac file-name" v-for="(item, index) in yunFileArr" :key='index' @click='yunFileUserEvent'>
                <bui-image src="/image/test.jpg" width="52px" height="52px"></bui-image>
                <div class="flex-jc">
                  <text class="f24 c51 fw4 mt17 lines2">121233211231323312</text>
                </div>
              </div>
            </scroller>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        yunFileArr: ['', '', '', '', '','']
      }
    },
    methods: {
      // 更多
      yunFileMoreEvent() {
        this.$alert()
      },
      // 常用联系人
      yunFileUserEvent() {
        this.$alert()
      }
    }
  }
</script>

<style lang="css" src="../css/common.css"></style>
<style>
  .main {
    flex: 1;
    padding-top: 100px;
    background-color: #666;
  }

  .yun-file {
    background-color: #fff;
  }

  .yun-file-title {
    height: 40px;
    margin: 18px 23px 36px 25px;
  }

  .yun-file-content {
    padding: 0 24px;
  }

  .file-name {
    margin-right: 15px;
    width: 166px;
  }

  .yun-scroller {
    width: 792px;
    height: 140px;
  }
</style>