<template>
  <div class="game">
    <div class="photo-box">
      <div class="photo-box-inner"></div>
      <div :class="photoBoxInner1"></div>
      <div :class="photoBoxInner2"></div>
      <div class="photo-target" ref="target"></div>
      <div :class="photoBoxInner"></div>
      <div class="time" v-show="isshowtime">
        {{time}}
      </div>
      <div class="photo-btn" @click="dophoto">
        <div class="photo-btn-inner">
          pai
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'game',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      time: 3,
      si: null,
      isshowtime: true,
      ismove: false,
      photoBoxInner: 'photo-box-inner-flash',
      photoBoxInner1: 'photo-box-inner',
      photoBoxInner2: 'photo-box-inner',
      detaTargetRotate: 10,
      startTargetLeft:0,
      endTargetLeft:0,
      detaTargetLeft:0.1,
      detaTargetTop:0.1,
      endTargetTop:0,
      centerLeft: 3.25,
      centerTop: 3.5,
      score: 0,
      started: false,
    }
  },
  methods: {
    dophoto() {

      if (this.ismove == true) {
        this.ismove = false
      }
      
    },
    render() {
      // 一些更新界面的操作
      this.setTarget()
      if(this.ismove){
        requestAnimationFrame(this.render);
      }else{
        this.photoBoxInner = 'photo-box-inner-flash colorani'
        this.photoBoxInner1 = 'photo-box-inner rotateani1'
        this.photoBoxInner2 = 'photo-box-inner rotateani2'
        if (this.endTargetTop > 7) {
          this.score = 0
        } else {
          let dx = this.centerLeft - this.endTargetLeft - 0.5
          let dy = this.centerTop - this.endTargetTop - 0.5
          let dl = Math.pow(dx * dx + dy * dy,0.5)
          
          this.score = 100 - dl * 10
          
        }
        
        sessionStorage.setItem('score',this.score.toFixed(0))
          let vm = this
          setTimeout(function(){
            vm.$router.push('/over')
          },1200)

      }
      
    },
    setTarget() {
      let target = this.$refs.target
      let left = target.style.left
      let top = target.style.top

      if (left == '') {
        left = this.startTargetLeft + 'rem'
      }
      if (top == '') {
        top = '-2rem'
      }

      let leftnum = parseFloat(left.substring(0,left.length-3))
      let topnum = parseFloat(top.substring(0,top.length-3))
      left = (leftnum + this.detaTargetLeft).toFixed(2) + 'rem'
      top = (topnum + this.detaTargetTop).toFixed(2) + 'rem'

      this.endTargetLeft = leftnum
      this.endTargetTop = topnum

      if(topnum > 13) {
        this.ismove = false
      }

      let targetRotate = target.style.transform;
      if (targetRotate == '') {
        targetRotate = 'rotate(0deg)'
      }
      let targetRotateNum = (parseFloat(targetRotate.substring(7,9))) > 360 ? 0 : (parseFloat(targetRotate.substring(7,9)) + this.detaTargetRotate)
      target.style.transform='rotate(' + targetRotateNum + 'deg)'

      target.style.left = left
      target.style.top = top
      
    },
    RandomNumBoth(Min,Max) {
      var Range = Max - Min;
      var Rand = Math.random();
      var num = Min + Math.round(Rand * Range); //四舍五入
      return num;
    }
  },
  mounted() {
    let vm = this
    vm.startTargetLeft = vm.RandomNumBoth(1,6)
    if (vm.startTargetLeft > 3) {
      vm.endTargetLeft = vm.RandomNumBoth(1,3)
    } else {
      vm.endTargetLeft = vm.RandomNumBoth(4,6)
    }
    vm.detaTargetLeft = (vm.endTargetLeft - vm.startTargetLeft)*0.02
    vm.detaTargetTop = (7+2)*0.02
    vm.detaTargetRotate = vm.RandomNumBoth(0,10)
    vm.si = setInterval(function(){
          if(vm.time > 1){
            vm.time -= 1
          } else {
            clearInterval(vm.si)
            vm.isshowtime = false
            vm.ismove = true
            vm.render()
          }
        },1000)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.game{
  width: 100%;
  height: 100%;
  padding-top: 1rem;
  box-sizing: border-box;
  background: #7F58C2;
}
.photo-box{
  width: 100%;
  height: 7rem;
  background: #EDD9FC;
  padding-top: .2rem;
  box-sizing: border-box;
  position: relative;
}
.photo-box-inner{
  width: 95%;
  height: 90%;
  background: #7C51AA;
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  transform-origin: 0;
  border:1px solid white;
}
.photo-box-inner-flash{
  width: 95%;
  height: 90%;
  background: rgba(255,255,255,0);
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
.photo-target{
  width:1rem;
  height: 1rem;
  background: yellow;
  text-align: center;
  line-height: 1rem;
  position: absolute;
  left: 0rem;
  top: -2.5rem;
}
.photo-btn{
  width: 1rem;
  height: 1rem;
  border-radius: 50%;
  border:2px solid #FFFCAD;
  background: rgba(255,255,255,0);
  position: absolute;
  left: 50%;
  bottom: 0;
  transform: translate(-50%,50%);
}
.photo-btn-inner{
  width: 90%;
  height: 90%;
  border-radius: 50%;
  background: rgba(124,81,170,0.6);
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  text-align: center;
  line-height: .85rem;
  color: #FFFCAD;
}
.time{
  width: .5rem;
  height: .5rem;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
}
.colorani{
  animation:colorani .25s;
}
@keyframes colorani {
  0%{
    background: rgba(255,255,255,0);
  }
  50%{
    background: rgba(255,255,255,0.6);
  }
  100%{
    background: rgba(255,255,255,0);
  }
}
.rotateani1{
  animation:rotate1 .75s .25s forwards;
}
@keyframes rotate1{
  0%{
    transform: rotate(0deg) translateX(-50%);
  }
  100%{
    transform: rotate(2.5deg) translateX(-50%);
  }
}
.rotateani2{
  animation:rotate2 .75s .25s forwards;
}
@keyframes rotate2{
  0%{
    transform: rotate(0deg) translateX(-50%);
  }
  100%{
    transform: rotate(5deg) translateX(-50%);
  }
}
</style>
