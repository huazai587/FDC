<template>
<div>
  <TopNav></TopNav>
     <!--主体 st-->
    <!--全部作品st -->
    <div class="all_zuop">
        <!--选择头 st-->
       <div class="choice_wrap">
            <ul class="choice_xuan">
                <li>
                    <a  v-for="(num,index) in projects" @click="iscur = index,searchList(index,'lei1')"  :class="{active:iscur==index}" :key="num.index" >{{num.titleTab }}</a>
                </li>
                <li style="margin-bottom:0">
                    <a  v-for="(num,index) in numsType" @click="iscur1 = index,searchList(index,'lei2')"  :class="{active:iscur1==index}" :key="num.index" >{{num.titleTab }}</a>
                </li>

            </ul>

        </div>
        <!--选择头 end-->
        <div class="content">
            <!-- 瀑布流开始 -->
           <vue-waterfall-easy :imgsArr="imgsArr" @scrollLoadImg="fetchImgsData">
               <template slot-scope="props">
                <p class="description"><a  :href="'/#/detail_share_uiix/'+ props.value.id" target="_blank">{{ props.value.title }}</a></p>
                <div class="qianm clearfloat">
                 <span class="sp1">作者：{{props.value.author }}</span>
                   <span class="sp3">{{ props.value.shareTime }}</span>
                 </div>
              </template>
            </vue-waterfall-easy>
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
const vueWaterfallEasy = () => import ('@/components/vue-waterfall-easy')
export default {
  name:'list_uiix',
  components:{
    TopNav,TopFan,vueWaterfallEasy
  },
  created () {
        this.getSubject(),
        this.imgsArr = this.initImgsArr()
  },
  data () {
        return {
          stories: [],
          projects:[
              {titleTab:'全部'},
              {titleTab:'房app'},
              {titleTab:'搜房帮'},
              {titleTab:'OA'},
              {titleTab:'工作台'},
              {titleTab:'其他资源'}
          ],
          numsType:[
              {titleTab:'全部'},
              {titleTab:'界面'},
              {titleTab:'插画'},
              {titleTab:'图标'},
              {titleTab:'引导页'}
          ],
          iscur:0 ,
          iscur1:0 ,
          typeIndex:0,
          typesIndex:0,
          index:0,
          searchData:{
              type1:0,
              type2:0
          },
          n:1,
          m:0,
          nn:1,
          pagesize:10,
          total:0,
          imgsArr: [],         //存放所有已加载图片的数组（即当前页面会加载的所有图片）
          fetchImgsArr: []
        }
      }, 
     
      methods: {
        //   获取数据
        getSubject () {
        this.$http.get(`${this.$url}/?c=index&a=showAppList&from=index`)
       .then(res => {
                   this.stories = res.data.errmsg,
                   this.total = res.data.total;
                })
                .catch(e => {
                  console.log(e)
                })
        },
        //   pubuliu
          initImgsArr(n) { 
          let arr = [];
           $.ajax({
            type:'get',
            url:'https://bird.ioliu.cn/v1/?url=http://testued.light.fang.com/?c=index&a=showAppList&from=index&page=' + n,
            contentType: "application/json;charset=utf-8",
            data:{
               'type1':this.searchData.type1,
               'type2':this.searchData.type2,
               'pagesize':this.pagesize
            },
            dataType: "json",
            async: false,
            success: function(data) {
              this.stories = data.errmsg;
                for (let i =0; i <this.stories.length; i++) {
                    arr.push({ 
                    src: this.stories[i].coverImg,
                    author: this.stories[i].author, 
                    title: this.stories[i].title,
                    shareTime:this.stories[i].shareTime,
                    id:this.stories[i].id,
                    link1:'/#/detail_share_uiix/' + this.stories[i].id
                    }) 
                };
            }     
          });
        //   console.log(arr)
            return arr ;

          },
           fetchImgsData() {
                //  console.log(this.total)
                if(this.m< Math.ceil(this.total/this.pagesize)){
                this.m = (this.n)++;
                this.fetchImgsArr = this.initImgsArr(this.m),
                this.imgsArr = this.imgsArr.concat(this.fetchImgsArr)
                }else{
                  $('#more').show()
                }
            },
             // 筛选函数
        searchList(obj,type){
             let that = this;
            //  let arr = [];
             switch(type){
                case 'lei1':
                this.searchData.type1 = obj;
                break;
                case 'lei2':
                this.searchData.type2 = obj;
                break;
                
             };
              this.imgsArr = this.initImgsArr(that.nn)
           }
       
        
        }
}
</script>

