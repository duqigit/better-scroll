<template>
  <div>
      <div class="wrapper" ref="wrapper" :pullUpLoad="pullUpLoad">
        <ul class="content" style="background-color:white">
            <p class="pulldown" v-if="pulldown.pulldowns"><span class="iconfont icon-shangla"></span>{{pulldown.message}}</p>
            <li v-for="item in startnumber">我是第{{item.number}}个</li>
            <p class="pulldown" v-if="pullup.pullup">{{pullup.message}}</p>
        </ul>
      </div>
      <button @click="btn">点击</button>
      <br/>
  </div>
</template>

<script>
import axios from '~/plugins/axios'
import BScroll from '~/plugins/scroll'
export default {
  async asyncData () {
    let { data } = await axios.get('/api/users')
    return { users: data }
  },
  data(){
    return {
      scroll:"",
      pulldown:{
        message:"下拉刷新",
        iconfont:"",
        pulldowns:false
      },
      pullup:{
        message:"下拉加载更多",
        pullup:false
      },
      startnumber:[{
          number:1
        },{
          number:2
        },{
          number:3
        },{
          number:4
        },{
          number:1
        },{
          number:2
        },{
          number:3
        },{
          number:4
        },{
          number:1
        },{
          number:2
        },{
          number:3
        },{
          number:4
        },{
          number:1
        },{
          number:2
        },{
          number:3
        },{
          number:4
        },{
          number:1
        },{
          number:2
        },{
          number:3
        },{
          number:4
        },{
          number:1
        },{
          number:2
        },{
          number:3
        },{
          number:4
        }]
    }
  },
  head () {
    return {
      title: 'Users'
    }
  },
  methods:{
    btn(){
      this.scroll.scrollTo(0,50,1000,'ease');
    },
    goToPage(){
      this.scroll.goToPage(0,2,1000,'ease');
    },
    getCurrentPage(){
      console.log(this.scroll.getCurrentPage())
    },
    pullUpLoad(){
       this.scroll.finishPullDown()
    },
    maxScrollX(){
      alert(this.scroll.maxScrollY);
    },
    next(){
      this.scroll.next();
    },
    finishPullDown(){
      this.scroll.finishPullDown();
    },
    pullDownRefresh(){
      alert(11)
    }
  },
  mounted(){
     this.$nextTick(() => {
          this.pulldown.pulldowns=true;
          this.scroll = new BScroll(this.$refs.wrapper,{
            click:true,
            probeType:1,
            startY:-50,
            pullDownRefresh: true,
             pullUpLoad:{
                  threshold: 0, // 负值是当上拉到超过低部 70px；正值是距离底部距离 时，
              }
          })
          this.scroll.on("scrollEnd",()=>{
              if(this.scroll.y==0){
                 this.scroll.scrollTo(0,-50,1000,'ease');
              }
          })
          this.scroll.on("pullingDown",()=>{
            this.pulldown.message="刷新中....";
            let that=this;
            new Promise(function(resolve, reject) {
              console.log("ga");
              setTimeout(function(){
                for (var i = 0;i < 20; i++) {
                  that.startnumber.push({number:i})
                }
                that.pulldown.message="刷新成功";
                resolve();
              },2000)
            }).then(function(value) {
              that.scroll.scrollTo(0,-50,1000,'ease');
              that.scroll.finishPullDown();
              that.scroll.refresh();
              that.pulldown.message="下拉刷新";
            }, function(value) {

            });
          })
          this.scroll.on("pullingUp",(e)=>{
            let that=this;
            this.pullup.pullup=true;
            that.pullup.message="正在加载...";
            // let that=this;
            new Promise(function(resolve, reject) {
              setTimeout(function(){
                for (var i = 0;i < 20; i++) {
                  console.log(1111);
                  that.startnumber.push({number:i})
                }
                resolve();
              },2000)
            }).then(function(value) {
              // that.scroll.scrollTo(0,-50,1000,'ease');
              that.scroll.finishPullUp();
              that.scroll.refresh();
              // that.pulldown.message="下拉刷新";
            }, function(value) {

            });
          })
        })
  },
  created (){

  }
}
</script>

<style scoped>
*{
  margin: 0;
  padding: 0;
}
.wrapper{
  width: 100%;
  height: 400px;
  overflow: hidden;
}
.pulldown{
  width: 100%;
  text-align: center;
  height: 50px;
  line-height: 50px;
}
</style>
