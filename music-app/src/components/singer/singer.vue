<template>
  <div class="singer">
      
  </div>
</template>

<script type="text/ecmascript-6">
  import {getSingerList} from 'api/singer'
  import {ERR_OK} from 'api/config'
  import Singer from 'common/js/singer'

  const HOT_NAME = "热门"
  const HOT_SINGER_LEN = 10

  export default {
    data(){
      return{
        singers:[]
      }
    },
    created(){
      this._getSingerList();
    },
    methods:{
      _getSingerList(){
        getSingerList().then((res) => {
          if(res.code === ERR_OK){
            this.singers = res.data.list;
            console.log(this._normalizeSinger(res.data.list));
          }
        })
      },
      _normalizeSinger(list){
        let map = {
          hot:{
            title: HOT_NAME,
            items: []
          }
        }
        list.forEach((item,index) => {
          if(index < HOT_SINGER_LEN){
            map.hot.items.push(new Singer({
              id:item.Fsinger_mid,
              name:item.Fsinger_name
            }))
          }
          const key = item.Findex
          if(!map[key]){
            map[key] = {
              title:key,
              items:[]
            }
          }
          map[key].items.push(new Singer({
            id:item.Fsinger_mid,
            name:item.Fsinger_name
          }))
        })
        //为了得到有序列表，我们需要处理map
        let hot = [];
        let ret = [];
        for(let key in map){
          let val = may[key];
          if(val.title.mattch(/[a-zA-Z]/)){
            res.push(val)
          }else if(val.title === 'HOT_NAME'){
            hot.push(val);
          }
        }
        res.sort((a,b) => {
          return a.title.chartCodeAt(0) - b.title.chartCodeAt(0);
        })
        return hot.concat(ret);
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  .singer
    position fixed
    top 88px
    bottom 0
    width 100%
</style>
