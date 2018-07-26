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
            <div class="waterfull clearfloat" >
                <ul>
                    <li class="item" v-for="story in PcLists" :key="story.id" @click="getTuList(story.id)">
                         <a  class="a-img" href="javascript:;">
                           <img :src="story.coverImg"  v-lazy="story.coverImg" />
                        </a>
                        <p class="description"><a  href="javascript:;">{{ story.title }}</a></p>
                        <div class="qianm clearfloat">
                            <span class="sp1">作者：{{story.author}}</span>
                            <span class="sp3">{{story.publishTime}}</span>
                        </div>
                    </li>

                </ul>
            </div>
           
        </div>
    </div>
    <!--全部作品end -->
     <!-- 分页 -->
        <Page :total="total" :current-page='current' @pagechange="pagechange"  :get-parent="typeIndex"></Page>
    <!--主体 end-->
        <!--弹窗 st-->
    <div class="mask" style="display: none;"></div>
    <div class="popDetail pop_1"   >
        <div class="leftcolumn">
            <div class="picMain">
                <div class="body">
                    <div class="swiper-container">
                        <div class="swiper-wrapper">
                            <div class="swiper-slide"  v-for="imglist in tuLists.content" :key="imglist.index">
                                <img :src="imglist" alt=""  v-lazy="imglist">
                            </div>
                        </div>
                    </div>
                </div>
                <div class="op">
                    <a href="javascript:void(0);" class="ac_turn t_l ">上一页</a>
                    <a href="javascript:void(0);" class="ac_turn t_r">下一页</a>
                    <span class="red" style="display:none;position:absolute;bottom:18px;right:-20px;">+1</span>
                </div>
            </div>
        </div>
        <div class="rightcolumn">
            <p class="h2">{{tuLists.title}}</p>
            <p class="h3">
                <span class="time">{{tuLists.publishTime}}</span>
                <span>作者：{{tuLists.author}}</span>
            </p>
            <div class="ewm-box clearfix">
                <img :src="tuLists.qr_code" alt="" class="ewm">
                <p class="title-ewm"><i></i>扫一扫，手机欣赏</p>
            </div>
        </div>
        <a href="javascript:void(0);" class="pop-close"></a>
    </div>
   <div class="popDetail popDetail_small pop_2">
        <div class="leftcolumn">
            <div class="picMain">
                <div class="body">
                    <div class="swiper-container">
                        <div class="swiper-wrapper">
                            <div class="swiper-slide"  v-for="imglist in tuLists.content" :key="imglist.index">
                                <img :src="imglist" alt=""  v-lazy="imglist">
                            </div>
                        </div>
                    </div>
                </div>
                <div class="op">
                    <a href="javascript:void(0);" class="ac_turn t_l ">上一页</a>
                    <a href="javascript:void(0);" class="ac_turn t_r">下一页</a>
                    <span class="red" style="display:none;position:absolute;bottom:18px;right:-20px;">+1</span>
                </div>
            </div>
        </div>
        <div class="rightcolumn">
            <p class="h2">{{tuLists.title}}</p>
            <p class="h3">
                <span class="time">{{tuLists.publishTime}}</span>
                <span>作者：{{tuLists.author}}</span>
            </p>
            <div class="ewm-box clearfix">
                <img :src="tuLists.qr_code" alt="" class="ewm">
                <p class="title-ewm"><i></i>扫一扫，手机欣赏</p>
            </div>
        </div>
        <a href="javascript:void(0);" class="pop-close"></a>
    </div>
    <!--弹窗 end-->
    <!--返回顶部 st-->
      <topFan></topFan>
    <!--返回顶部 end-->
  </div>
</template>

<script>
const TopNav = () => import ('@/components/topnav')
const TopFan = () => import ('@/components/topFan')
const Page = () => import ('@/components/page')
import swiper from 'swipe'
export default {
  name:'list_h5',
  components:{
    TopNav,TopFan,Page
  },
  data () {
        return {
          PcLists: [],
          tuLists:{},
           total: 0,// 记录总条数
          display: 25,   // 每页显示条数
          current: 1, // 当前的页数 
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
              colorRange:''
          }
        }
      },
      created () {
            this.getList();
         this.$nextTick(function() {
             this.h5Pop()
         })
      },  
 
      methods: {
        //   弹窗特效
          h5Pop(){
            var $window = $(window);
            var ww = $window.width();
	        var hh = $window.height();
            var $mask = $('.mask');
            var $popDetail = $('.popDetail').eq(0);
            function setUl(){
                $('.pop-close').click();
                ww = $window.width();
                hh = $window.height();
                ww = ww>1160?ww:1160;
                $popDetail = hh>800?$('.popDetail').eq(0):$('.popDetail_small');
                }

                function __init(){
                    $('.pop-close').click();
                    ww = $window.width();
                    hh = $window.height();
                    ww = ww>1160?ww:1160;
                    $popDetail = hh>800?$('.popDetail').eq(0):$('.popDetail_small');
                }
                __init();
                $window.resize(function(){
                    setUl();
                });
            //detail pop
            $('body').on('click','.a-img img ',__detail);
            function __detail(){
                $popDetail.fadeOut();
                var $t = $(this);
                    $popDetail.fadeIn();
                    $mask.fadeIn();
                    var swiper_container = '.pop_1 .swiper-container',swiper_btn_next = '.pop_1 .t_r',swiper_btn_prev = '.pop_1 .t_l';
                    if($popDetail.is('.pop_2')){
                        swiper_container = '.pop_2 .swiper-container',swiper_btn_next='.pop_2 .t_r',swiper_btn_prev = '.pop_2 .t_l';
                    }
                    $('.swiper-container').each(function(){
                        var $t = $(this);
                        var $li = $t.find('.swiper-slide');
                        var len = $li.length;
                        var liheight = $li.eq(0).outerHeight(true);
                        if(len==1){
                            $(swiper_btn_next).addClass('swiper-button-disabled');
                        }
                        $t.find('.swiper-wrapper').css({
                            'height':len*liheight,
                            'position':'absolute',
                            'top':'0',
                            'left':'0',
                            'display':'block'
                        });
                        $li.css('height','auto');
                    }).css({'overflow':'visible'}); 
                    function __turnleft(){
                        var $t=$(this) ,
                            $list = $t.closest('.picMain').find('.swiper-container') , 
                            $ul = $list.find('.swiper-wrapper') 
                            ;
                        var liheight = $ul.find('.swiper-slide').eq(0).find('img').height()+7,
                            len = $ul.find('.swiper-slide').length,
                            ultop = $ul.css('top'),
                            ulheight = liheight*(len-2)
                        ;
                        if(len<2) return;
                        if($t.is('.swiper-button-disabled')) return;
                        if(parseInt(ultop) == -liheight){
                            if($ul.is(':animated')) return
                            $ul.animate({'top':'+='+liheight+'px'});
                            $t.addClass('swiper-button-disabled');
                            $t.next().removeClass('swiper-button-disabled');
                            return;
                            // $ul.animate({'top':'0px'});
                        }else{
                            if($ul.is(':animated')) return
                            $t.next().removeClass('swiper-button-disabled');
                            $ul.animate({'top':'+='+liheight+'px'});
                        }
                    }
                    $(swiper_btn_prev).click(__turnleft);
                
                    function __turnright(){
                        var $t=$(this) , 
                            $list = $t.closest('.picMain').find('.swiper-container') ,
                            $ul = $list.find('.swiper-wrapper') 
                            ;
                        var liheight = $ul.find('.swiper-slide').eq(0).find('img').height()+7,
                            len = $ul.find('.swiper-slide').length,
                            ultop = $ul.css('top'),
                            ulheight = liheight*(len-2)
                        ;
                        if(len<2) return;
                        if($t.is('.swiper-button-disabled')) return;
                        if(parseInt(ultop) == -parseInt(ulheight)){
                            if($ul.is(':animated')) return
                            $ul.animate({'top':'-='+liheight+'px'});
                            $t.addClass('swiper-button-disabled');
                            $t.prev().removeClass('swiper-button-disabled');
                            return;
                        }else{
                            if($ul.is(':animated')) return
                            $t.prev().removeClass('swiper-button-disabled');
                            $ul.animate({'top':'-='+liheight+'px'});
                        }
                    }
                    $(swiper_btn_next).click(__turnright);

            };
            var $pop_close = $('.pop-close');
            $('body').on('click','.pop-close',function(){
                $popDetail.fadeOut();
                $mask.fadeOut();
            });
            $mask.click(function(){
                $pop_close.click();
            });
            },
          getList () {
          this.$http.get(`${this.$url}/?c=index&a=showH5List&from=index&pagesize=${this.display}`).then((res) => {
            this.PcLists = res.data.errmsg;
             this.total = res.data.total;
            })
             .catch(e => {
                  console.log(e)
                })
        },
        // 筛选函数
        searchList(obj,type){
            let that = this;
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
            }
           this.$http.get(`${this.$url}/?c=index&a=showH5List&page=current&from=index&pagesize=${this.display}`,{
               params:that.searchData
           })
          .then((res) => {
            that.PcLists = res.data.errmsg;
            this.total = res.data.total;
            })
            .catch(e => {
                  console.log(e)
            });
        },
        // 弹窗函数
         getTuList (storyId) {
            this.$http.get(`${this.$url}/?c=index&a=getOneH5Data&from=index&id=${storyId}`).then((res) => {
               this.tuLists = res.data.errmsg;
            })
            .catch(e => {
                  console.log(e)
            })
        },
          // 分页
      pagechange:function(currentPage){
            $('body,html').animate({
          scrollTop: $(".all_zuop").offset().top
        },10);
        this.current = currentPage;
        this.$http.get(`${this.$url}/?c=index&a=showH5List&page=${this.current}&from=index&pagesize=${this.display}`,{
             params:this.searchData
        })
            .then(res => {
                this.PcLists = res.data.errmsg;
                this.total = res.data.total;
            })
            .catch(e => {
            console.log(e)
        })
    },
        
    }
}
</script>


