<template>
  <div ref="wrap">
    <!-- 云盘 -->
    <div class="yun-file">
      <div class="yun-file-title flex">
        <text class="f28 fw5 c0">云盘</text>
        <text class="f24 c153 fw4 pl20 pt10 pb10" @click="yunFileMoreEvent">全部</text>
      </div>
      <div class="yun-file-content">
        <div class="pb35 bb">
          <text class="f24 fw4 c102">最近使用的文档</text>
          <div class="mt36 file-height">
            <div class="flex" v-if='isShowTM'>
              <scrolle v-if='yunFileReleArr.length != 0' class="yun-scroller flex-dr" show-scrollbar='false'
                scroll-direction="horizontal">
                <div class="flex-ac file-name" v-for="(item, index) in yunFileReleArr" :key='index'
                  @click='yunFileUserEvent(item.id)'>
                  <bui-image src="/image/sleep.png" width="52px" height="52px"></bui-image>
                  <div class="flex-jc" style="width: 153px">
                    <text class="f24 c51 fw4 mt17 lines2">123312123321321</text>
                  </div>
                </div>
              </scrolle>
              <div class="no-file flex-ac flex-jc" v-if='yunFileReleArr.length == 0'>
                <div class="flex-dr">
                  <bui-image src="/image/sleep1.png" width="42px" height="39px"></bui-image>
                  <text class="f26 c51 fw4 pl15 center-height ">{{isErrorBM?'暂无文档':'加载失败'}}</text>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="pb35 bb mt31">
          <text class="f24 fw4 c102">我分享的文档</text>
          <div class="mt36 file-height">
            <div class="flex" v-if='isShowBM'>
              <scroller v-if='yunFileByMeArr.length != 0' class="yun-scroller flex-dr" show-scrollbar='false'
                scroll-direction="horizontal">
                <div class="flex-ac file-name" v-for="(item, index) in yunFileByMeArr" :key='index'
                  @click='yunFileUserEvent(item.id)'>
                  <bui-image :src="item.image" width="52px" height="52px"></bui-image>
                  <div class="flex-jc" style="width: 153px">
                    <text class="f24 c51 fw4 mt17 lines2">{{item.name}}</text>
                  </div>
                </div>
              </scroller>
              <div class="no-file flex-ac flex-jc" v-if='yunFileByMeArr.length == 0'>
                <div class="flex-dr">
                  <bui-image src="/image/sleep1.png" width="42px" height="39px"></bui-image>
                  <text class="f26 c51 fw4 pl15 center-height ">{{isErrorBM?'暂无文档':'加载失败'}}</text>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="pb35 mt31">
          <text class="f24 fw4 c102">分享给我的文档</text>
          <div class="mt36 file-height">
            <div class="flex" v-if='isShowTM'>
              <scroller v-if='yunFileToMeArr.length != 0' class="yun-scroller flex-dr" show-scrollbar='false'
                scroll-direction="horizontal">
                <div class="flex-ac file-name" v-for="(item, index) in yunFileToMeArr" :key='index'
                  @click='yunFileUserEvent(item.id)'>
                  <bui-image :src="item.image" width="52px" height="52px"></bui-image>
                  <div class="flex-jc" style="width: 153px">
                    <text class="f24 c51 fw4 mt17 lines2">{{item.name}}</text>
                  </div>
                </div>
              </scroller>
              <div class="no-content flex-ac flex-jc" v-if='yunFileToMeArr.length==0'>
                <div class="flex-dr flex-jc">
                  <bui-image src="/image/sleep1.png" width="42px" height="39px"></bui-image>
                  <text class="f26 c51 fw4 pl15 center-height ">{{isErrorTM?'暂无任务':'加载失败'}}</text>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  const link = weex.requireModule("LinkModule");
  const linkapi = require("linkapi");
  const dom = weex.requireModule('dom');
  export default {
    data() {
      return {
        yunFileByMeArr: [],
        yunFileToMeArr: [],
        yunFileReleArr: [],
        isErrorBM: true,
        isErrorRele: true,
        isErrorTM: true,
        isShowBM: false,
        isShowTM: false,
        isShowRE: false,
        channel: new BroadcastChannel('WidgetsMessage')
      }
    },
    methods: {
      yunFileMoreEvent() {
        link.launchLinkService(['[OpenBuiltIn] \n key=MyDisk'], (res) => {}, (err) => {});
      },
      yunFileUserEvent(id) {
        link.launchLinkService(['[OpenBuiltIn] \n key=DiskDetail \n diskId=' + id], (res) => {}, (err) => {});
      },
      getFileImages(ext) {
        let fileImageTypes = {
          'excel': ['xls', 'xlsx'],
          'music': ['mp3', 'wma', 'wav', 'mod', 'ogg', 'm4a'],
          'pdf': ['pdf'],
          'photo': ['bmp', 'gif', 'jpeg', 'jpg', 'svg', 'png', 'psd'],
          'ppt': ['ppt', 'pptx'],
          'txt': ['txt', 'key'],
          'video': ['rm', 'rmvb', 'wmv', 'avi', 'mp4', '3gp', 'mkv', 'flv', 'mov', 'mpg'],
          'word': ['doc', 'docx', 'wps'],
          'zip': ['zip', 'rar', '7z'],
          'unknow': ['file'],
          'folder2': ['folder'],
          'team': ['team']
        }
        let fileImages = {};
        let fileTypeImages = {};
        for (let fext in fileImageTypes) {
          fileImages[fext] = '/image/' + fext + '.png';
          let arr = fileImageTypes[fext];
          if (arr.length > 0) {
            for (let i = 0; i < arr.length; i++) {
              fileTypeImages[arr[i]] = fext;
            }
          }
        }
        let type = fileTypeImages[ext];
        if (type) {
          return fileImages[type];
        } else {
          return fileImages['unknow'];
        }
      },
      getYunFile() {
        link.getServerConfigs([], (params) => {
          link.getLoginInfo([], (user) => {
            let objDataBy = {
              by: user.userId,
              to: '',
              scope: ''
            }
            let objDataTo = {
              by: '',
              to: 'U' + user.userId,
              scope: ''
            }
            linkapi.get({
              url: 'https://pan.bingosoft.net/openapi//file/share/list',
              data: objDataBy
            }).then((res) => {
              this.isShowBM = true
              this.isErrorBM = true
              let fileArr = []
              for (let index = 0, resLength = res.rows.length; index < resLength; index++) {
                let fileObj = {}
                const element = res.rows[index];
                fileObj['name'] = element.name
                fileObj['id'] = element.fileId
                if (element.type == 'D') {
                  fileObj['image'] = '/image/word.png'
                } else {
                  fileObj['image'] = this.getFileImages(element.extension)
                }
                fileArr.push(fileObj)
              }
              this.yunFileByMeArr = fileArr
              this.broadcastWidgetHeight()
            }, (err) => {
              this.isShowBM = true
              this.isErrorBM = false
              this.broadcastWidgetHeight()
            })

            linkapi.get({
              url: 'https://pan.bingosoft.net/openapi//file/share/list',
              data: objDataBy
            }).then((res) => {
              this.isShowTM = true
              this.isErrorTM = true
              let fileArr = []
              for (let index = 0, resLength = res.rows.length; index < resLength; index++) {
                let fileObj = {}
                const element = res.rows[index];
                fileObj['name'] = element.name
                fileObj['id'] = element.fileId
                if (element.type == 'D') {
                  fileObj['image'] = '/image/word.png'
                } else {
                  fileObj['image'] = this.getFileImages(element.extension)
                }
                fileArr.push(fileObj)
              }
              this.yunFileToMeArr = fileArr
              this.broadcastWidgetHeight()
            }, (err) => {
              this.isShowTM = true
              this.isErrorTM = false
              this.broadcastWidgetHeight()
            })
          }, (err) => {
            this.error()
          })
        }, () => {
          this.error()
        });
      },
      error() {
        this.isShowTM = true
        this.isShowBM = true
        this.isShowRE = true
        this.isErrorBM = false
        this.isErrorRele = false
        this.isErrorTM = false
        this.broadcastWidgetHeight()
      },
      broadcastWidgetHeight() {
        let _params = this.$getPageParams();
        setTimeout(() => {
          dom.getComponentRect(this.$refs.wrap, (ret) => {
            this.channel.postMessage({
              widgetHeight: ret.size.height,
              id: _params.id
            });
            channel.close();
          });
        }, 100)
      }
    },
    mounted() {
      this.channel.onmessage = (event) => {
        if (event.data.action === 'RefreshData') {
          this.getYunFile()
        }
      }
      this.getYunFile()
    }
  }
</script>

<style lang="css" src="../css/common.css"></style>
<style>
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
    width: 720px;
    height: 140px;
  }

  .no-file {
    height: 166px;
    width: 750px
  }

  .no-content {
    height: 166px;
    width: 750px
  }

  .center-height {
    line-height: 40px;
  }
</style>