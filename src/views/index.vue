<template>
  <div class="container" ref="container" style="position:relative;">
    <div class="surfaceBlub" v-show="imgIstrue" ></div>
    <img src="./businessFloor02.jpg" alt="办公区" usemap="#floor" ref="imgPrivite" id="baseImg" :class="{scale:imgIstrue}" @click="imgScale()">
    <map id="floor" name="floor">
      <area shape="poly" v-on:click="clickArea('print', $event)" coords="220,35,260,35,260,75,220,75" alt="多边形" id="room01print">
      <area shape="poly" v-on:click="clickArea('tv', $event)" coords="465,280,480,280,480,400,465,400"  alt="多边形" id="room01tv">
      <area shape="poly" v-on:click="clickArea('drink', $event)" coords="435,490,480,490,480,530,435,530"  alt="多边形" id="room01drink">
    </map>
  </div>
</template>

<script type="text/ecmascript-6">
import $ from 'jquery'
/* eslint-disable */
export default {
  name: 'index',
  // eslint-disable-next-line
  data () {
    return {
      clickId: 0,
      idArrey: [],
      eleLeft: 0,
      eleTop: 0,
      imgIstrue:false, /*定义数组*/
      imgArrey: []
    }
  },
  methods: {
    // getData: function(){ /*这个方法是我自己定义的，作用：为了拿到后台数据*/
    //   const row = 10; /*这个是后台拿到的数据条数，可以根据length获取*/
    //   for(let i =0;i<row ;i++) { this.imgIstrue[i] = false; /*初始化为false*/ } },
    imgScale:function () { /*这个方法是为了标识改变被点击图片的状态*/
      const isTrue = this.imgIstrue; /*获取被点击元素当前的值 false或true*/
      // const that = this
      // // this.imgIstrue.forEach(function (val,row) {
      // //   that.imgIstrue.splice(row,1,false) /*刷新数组，将数组所有值设置为false，即图片原始状态*/
      // // })
      // this.imgIstrue.splice(index,1,!isTrue) /*将被点击数组元素的值反转 用于记录状态的改变*/
      this.imgIstrue = !isTrue
    },
    clickArea: function(name, event) {
      const id = event.target.id
      const idExit = this.imgArrey.indexOf(id)
      if (this.imgIstrue === true && idExit === -1){
        console.log(this.$refs.imgPrivite)
        console.log('点击到: ', name, event)
        const distance = event.target.coords.replace('"', '').split(',')
        this.eleLeft = parseInt(distance[0])-30
        this.eleTop = parseInt(distance[1])-20
        const str = `<div style="left:${this.eleLeft}px; top: ${this.eleTop}px; position: absolute;  z-index: 1000"><img id = "img_` + id + `" class="imgblub" src="../../static/bulb.png"  style = "height: 50px; width: 50px;" ></div>`
        this.imgArrey.push(id)
        const that = this
        $(document).ready(function() {
          $('.surfaceBlub').append(str)
          $('.imgblub').click(function() {
            console.log('点击的img', this)
            $('#' + this.id).remove()
            const nowExit = that.imgArrey.indexOf(id)
            that.imgArrey.splice(nowExit, 1)
          })
        })
      } else {
        console.log('图片为缩小状态', this.imgIstrue)
      }
    },
  }
}
</script>

<style >
  @import '../styles/reset.css';
  #baseImg { transform: scale(0.5); /*图片原始大小1倍*/
    transition: all ease 0.5s; } /*图片放大所用时间*/
  #baseImg.scale { transform: scale(1); /*图片需要放大2倍*/
    position: relative; /*相对定位，是相对于前面的容器定位的*/
    /*z-index: 100; !*属性设置元素的堆叠顺序,保证图片放大后处于最上层*!*/
  }
</style>
