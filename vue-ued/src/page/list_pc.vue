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
                    <a  v-for="(num,index) in years" @click="iscur = index,searchList(num.titleTab,'year')"  :class="{active:iscur==index}" :key="num.index" >{{num.titleTab }}</a>
                </li>
                <li>
                    <a  v-for="(num,index) in departs" @click="iscur1 = index,searchList(index,'depart')"  :class="{active:iscur1==index}" :key="num.index" >{{num.titleTab }}</a>
                </li>
                <li class="sexi">
                    <a  v-for="(num,index) in colors" @click="iscur2 = index,searchList(index,'color')"  :class="{active:iscur2==index}" :key="num.index" >{{num.titleTab }}</a>

                </li>
            </ul>

        </div>
        <!--选择头 end-->
        <div class="content">
           <vue-waterfall-easy :imgsArr="imgsArr" @scrollLoadImg="fetchImgsData">
               <template slot-scope="props">
                <p class="description"><a :href="props.value.link" target="_blank">{{ props.value.title }}</a></p>
                <div class="qianm clearfloat">
                 <span class="sp1">作者：{{props.value.author }}</span>
                   <span class="sp3">{{props.value.publishTime}}</span>
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
  name:'list_pc',
  components:{
    TopNav,TopFan,vueWaterfallEasy
  },
  created () {
        this.getSubject();
        this.imgsArr = this.initImgsArr()
  },
 
  data () {
        return {
          PcLists: [],
          years:[
              {titleTab:'全部'},
              {titleTab:'2016'},
              {titleTab:'2017'},
              {titleTab:'2018'}
          ],
          departs:[
              {titleTab:'全部'},
              {titleTab:'新房DL'},
              {titleTab:'新房AI'},
              {titleTab:'平台运营'},
              {titleTab:'二媒'},
              {titleTab:'二电'},
              {titleTab:'市场部'},
              {titleTab:'其它部门'}
          ],
          colors:[
               {titleTab:'全部'},
               {titleTab:''},
               {titleTab:''},
               {titleTab:''},
               {titleTab:''},
               {titleTab:''},
               {titleTab:''},
               {titleTab:''},
               {titleTab:''},
               {titleTab:''},
               {titleTab:''},
               {titleTab:''},
          ],
          iscur:0 ,
          iscur1:0 ,
          iscur2:0 ,
          typeIndex:0,
          typesIndex:0,
          index:0,
          searchData:{
              selYear:'',
              ownGroup:'',
              colorRange:'',
          },
          n:1,
          m:0,
          nn:1,
          pagesize:10,
          total:0,
          imgsArr: [],         //存放所有已加载图片的数组（即当前页面会加载的所有图片）
          fetchImgsArr: []     //存放每次滚动时下一批要加载的图片的数组
        }
      },
      methods: {  
        //   获取数据
           getSubject () {
             this.$http.get(`${this.$url}/?c=index&a=showPcList&from=index`).then((res) => {
                        this.PcLists = res.data.errmsg,
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
            url:'https://bird.ioliu.cn/v1/?url=http://testued.light.fang.com/?c=index&a=showPcList&from=index&page=' + n,
            contentType: "application/json;charset=utf-8",
            data:{
               'selYear':this.searchData.selYear,
               'ownGroup':this.searchData.ownGroup,
               'colorRange':this.searchData.colorRange,
               'pagesize':this.pagesize
            },
            dataType: "json",
            async: false,
            success: function(data) {
              this.PcLists = data.errmsg;
                for (let i =0; i <this.PcLists.length; i++) {
                    arr.push({ 
                    src: this.PcLists[i].coverImg,
                    author: this.PcLists[i].author, 
                    title: this.PcLists[i].title,
                    publishTime:this.PcLists[i].publishTime,
                    link1:'/#/detail_share/' + this.PcLists[i].id
                    }) 
                };
            }     
          });
        //   console.log(arr)
            return arr ;
            },
            fetchImgsData() {
                if(this.m< Math.ceil(this.total/this.pagesize) ){
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
                case 'year':
                this.searchData.selYear = obj;
                break;
                case 'depart':
                this.searchData.ownGroup = obj;
                break;
                case 'color':
                this.searchData.colorRange = obj;
                break;
             };
              this.imgsArr = this.initImgsArr(that.nn)
           }
          }
        }
       
</script>
