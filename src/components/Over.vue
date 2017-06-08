<template>
  <div class="over">
    <div class="score-text">
      <div class="score">得分{{score}}<br>{{chance}}</div>
    </div>
    
    <div class="btn roll" @click="roll()">抽奖</div>
    <div class="btn restart" @click="restart()">再来一局</div>
    <div class="btn share" @click="ishowshare = !ishowshare">分享</div>
    <div class="layer" v-show="ishowshare" @click="ishowshare = !ishowshare">
      <div class="img-wrap clearfix">
        <img src="../assets/share-arrow.png" class="share-arrow">
      </div>
      点击右上角分享给好友
    </div>
  </div>
</template>

<script>
export default {
  name: 'over',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      score: 0,
      chance: '恭喜获得一次抽奖机会',
      ishowshare: false,
    }
  },
  mounted () {
    this.score = sessionStorage.getItem('score')
    if (parseInt(this.score) < 10) {
      this.chance = '很遗憾，当前得分不能参与抽奖'
    }
  },
  methods: {
    roll () {
      console.log('抽奖')
    },
    restart () {
      this.$router.push('/game')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.btn{
  padding: 5px;
  box-sizing: border-box;
  background: #6E3923;
  color: white;
  box-shadow:0 0 2px 1px #502A1A;
}
.roll,.restart,.share{
  width: 25%;
  margin: .25rem auto;
  text-align: center;
}
.score-text{
  width: 100%;
  height: 5rem;
  background: #FEF7CB;
  text-align: center;
  padding-top: 1rem;
}
.layer{
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  text-align: center;
  color: white;
  background: rgba(0,0,0,0.7);
}
.share-arrow{
  width:3.78rem;
  display: block;
  float: right;
}
.img-wrap{
  padding:.25rem;
}
.clearfix{
  overflow: hidden;
}
</style>
