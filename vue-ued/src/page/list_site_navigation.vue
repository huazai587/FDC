<template>
<div class="bmBox">
  <TopNav></TopNav>
     <!--主体 st-->
    <!--全部作品st -->
        <div class="choice_wrap">
        <ul class="choice_xuan">
              <li>
                  <div class="fl wzdh_title">设计</div>
                  <div class="fl">
                    <a  v-for="(num,index) in designs" @click="iscur = index,worksAll(num,index)"  :class="{active:iscur==index}" :key="num.index" >{{num.titleTab }}</a>
                   </div>
                </li>
                <li style="margin-bottom:0">
                    <div class="fl wzdh_title">制作</div>
                    <div class="fl">
                    <a  v-for="(num,index) in webs" @click="iscur1 = index,worksAllType(num,index)"  :class="{active:iscur1==index}" :key="num.index" >{{num.titleTab }}</a>
                     </div>
                </li>
        </ul>
        </div>
        <div class="fang_responsive">        
            <div class="listBox"  v-show="sjdh">
                <div class="tit">{{titleSj}}</div>
                <div class="con">
                    <div class="cBox">
                        <ul>
                            <li v-for="story in PcListsSj" :key="story.id"><i></i><a :href="story.link" target="_blank">{{story.title}}</a></li>
                        </ul>
                    </div>
                </div>
            </div>
            <div class="listBox"  v-show="qddh">
                <div class="tit">{{titleQd}}</div>
                <div class="con">
                    <div class="cBox">
                        <ul>
                            <li v-for="story in PcListsQd" :key="story.id"><i></i><a :href="story.link" target="_blank">{{story.title}}</a></li>
                        </ul>
                    </div>
                </div>
            </div>

        </div>
    
    <!--全部作品end -->
    <!--主体 end-->
    <!--返回顶部 st-->
      <topFan></topFan>
    <!--返回顶部 end-->
  </div>
</template>

<script>
const TopNav = () => import ('@/components/topnav')
const TopFan = () => import ('@/components/topFan')
export default {
  name:'list_site_navigation',
  components:{
    TopNav,TopFan
  },
  data () {
        return {
          PcListsSj: [],
          PcListsQd: [],
          designs:[
              {titleTab:'设计全部'},
              {titleTab:'设计导航'},
              {titleTab:'设计资讯'},
              {titleTab:'设计素材'},
              {titleTab:'图片助手'},
              {titleTab:'页面配色'},
              {titleTab:'动效收集'},
              {titleTab:'学习网站'},
              {titleTab:'字体'},
              {titleTab:'软件下载'},
              {titleTab:'其他'}
          ],
          webs:[
              {titleTab:'制作全部'},
              {titleTab:'H5实例'},
              {titleTab:'图片代码'},
              {titleTab:'导航菜单'},
              {titleTab:'选项卡'},
              {titleTab:'文字特效'},
              {titleTab:'表单代码'},
              {titleTab:'提示层/浮层/弹出层'},
              {titleTab:'其他'}
          ],
          iscur:0 ,
          iscur1:0 ,
          typeIndex:0,
          typesIndex:0,
          index:0,
          titleSj:'设计导航',
          titleQd:'制作导航',
          sjdh: true,
          qddh: true
        }
      },
      created () {
        this.getListSj(),
        this.getListQd()
      },
      methods: {
        getListSj () {
       this.$http.get(`${this.$url}/?c=index&a=showElseList&type1=1&from=index&pagesize=100`).then((res) => {
               this.PcListsSj = res.data.errmsg;
            })
             .catch(e => {
                  console.log(e)
                })
        },
        getListQd () {
       this.$http.get(`${this.$url}/?c=index&a=showElseList&type1=2&from=index&pagesize=100`).then((res) => {
               this.PcListsQd = res.data.errmsg;
            })
             .catch(e => {
                  console.log(e)
                })
        },
         // 设计切换展示
        worksAll (num,index) {
          this.typeIndex = index;
          this.titleSj = num.titleTab;
          this.iscur1 = -1;
          this.sjdh = true;
          this.qddh = false;
          this.$http.get(`${this.$url}/?c=index&a=showElseList&type1=1&type2=${this.typeIndex}from=index`)
          .then((res) => {
            this.PcListsSj = res.data.errmsg;
            })
            .catch(e => {
                  console.log(e)
            });
        },
        // 制作切换展示
        worksAllType (num,index) {
          this.typesIndex = index;
          this.titleQd = num.titleTab;
          this.iscur = -1;
           this.qddh = true;
          this.sjdh = false;
          this.$http.get(`${this.$url}/?c=index&a=showElseList&type1=2&type2=${this.typesIndex}&from=index`)
          .then((res) => {
            this.PcListsQd = res.data.errmsg;
            })
            .catch(e => {
                  console.log(e)
            });
        },
        }
}
</script>

