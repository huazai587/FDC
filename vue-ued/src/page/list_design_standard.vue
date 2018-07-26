<template>
<div class="bmBox">
  <TopNav></TopNav>
     <!--主体 st-->
  <div class="fang_responsive">        
        <div class="gfBox">
        	<div class="gfList">
            	  <a :href="item.link"  v-for="item in PcLists" :key="item.id" target="_blank" >{{ item.programName | dataFilter  }}</a>
            </div>
        </div>
    </div>
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
  name:'list_design_standard',
  components:{
    TopNav,TopFan
  },
   created () {
    this.getList()
  },
  data () {
    return {
    PcLists:[]
    }
  },
  filters: {
        dataFilter: function (value) { 
            if (!value) return ''
            value = value.toString().substr(0,10);
            return value;
        }
  },
  methods: {
    getList () {
    this.$http.get(`${this.$url}/?c=index&a=showRuleList&from=index`).then((res) => {
            this.PcLists = res.data.errmsg;
        //   console.log(res.data.recent)
        })
         .catch(e => {
                  console.log(e)
                })
    }
  }  
  }
</script>


