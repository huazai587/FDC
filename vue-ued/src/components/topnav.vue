<template>
  <div class="nav_wrap">
        <div class="nav fang_responsive clearfix">
            <div class="fl fang_logo">
                <router-link  to="/"><img src="../assets/images/fang_ued_logo.png" /></router-link>
            </div>
            <div class="fr nav_category" >
                <router-link  v-for="link in links"  :to="link.url" :key="link.index" :class="link.class">{{ link.title }}</router-link>
          </div>
        </div>
        <router-view></router-view>
    </div>
</template>

<script>
export default {
  name: 'TopNav',
  data () {
   return{
       links:[
           {
               title:'首页',
               url:'/',
               class:''
           },
           {
               title:'UI/UX',
               url:'/list_uiix',
               class:''
           },
           {
               title:'Pc专题',
               url:'/list_pc',
               class:''
           },
           {
               title:'Banner',
               url:'/list_banner',
               class:''
           },
           {
               title:'设计规范',
               url:'/list_design_standard',
               class:''
           },
           {
               title:'网址导航',
               url:'/list_site_navigation',
               class:''
           },
           {
               title:'H5',
               url:'/list_h5',
               class:'fang_nav_h5'
           },
           {
               title:'',
               url:'',
               class:'fang_nav_hui'
           },
       ]
   }
  },
}
</script>


<style >
@import "../style/common.css"
</style>
