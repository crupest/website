<template>
  <div class="Carousel">
    <div class="box">
      <div class="imgList">
        <transition name="fade" mode="out-in">
          <img
            @mouseenter="timestop"
            @mouseleave="timestart"
            :key="img.id"
            v-for="img in imgactive"
            :src="img.src"
            class="active"
          />
        </transition>
      </div>
      <div class="table">
        <a
          :key="item.id"
          @click.prevent="change(item.id)"
          v-for="item in imgSrcArr"
          :class="item.id == current_img_num ? 'active' : ''"
        ></a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MyCarousel",
  props: ["imgSrcArr", "intervalTime"],

  //接受两个参数,第一个为地址对象数组,须有id和src内容,src使用require()即可
  // imgSrcArr:[
  //             {
  //                 id:0,
  //                 src:require('../assets/logo.png'),
  //             },
  //             {
  //                 id:1,
  //                 src:require('../assets/me.jpg'),
  //             },
  //第二个参数为定时器时间,单位为ms

  data() {
    return {
      current_img_num: 0, //控制显示某一张图片的id,当current_img_num与id相等时候则显示
      timer_id: "", //接受Interval计时器的返回值,在mouted里面初始化
    };
  },
  methods: {
    change: function (id) {
      //点击按钮则重置计时器
      this.current_img_num = id;
      this.timestop();
      this.timestart();
    },
    timestop: function () {
      clearInterval(this.timer_id);
    },
    timestart: function () {
      this.timer_id = setInterval(() => {
        this.current_img_num++;
        if (this.current_img_num == this.imgSrcArr.length) {
          this.current_img_num = 0;
        }
      }, this.intervalTime);
    },
  },
  computed: {
    imgactive: function () {
      //考虑到v-if和v-for的冲突问题,在此使用计算属性来筛选
      var num = this.current_img_num;
      return this.imgSrcArr.filter(function (img) {
        return img.id == num;
      });
    },
  },
  mounted() {
    this.timestart();
  },
  beforeDestroy() {
    this.timestop();
  },
};
</script>
<style scoped>
* {
  margin: 0;
  padding: 0;
}
.Carousel {
  display: flex;
  justify-content: center;
}
.box {
  width: 600px;
  height: 300px;
  margin: 0 auto;
  position: absolute;
  overflow: hidden;
  text-align: center;
}
.imgList {
  display: inline-block;
  vertical-align: middle;
  position: relative;
}
.imgList img {
  border-radius: 5%;
  object-fit: cover;
  width: 300px;
  height: 300px;
}
.table {
  height: 0;
  width: 100%;
}
.table a {
  top: -42px;
  position: relative;
  display: inline-block;
  text-decoration: none;
  background: wheat;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  margin: 10px;
  opacity: 0.5;
}
.table .active {
  background: #42b983;
}
.fade-enter-active {
  transform: translateX(0px);
  opacity: 1;
  transition: all 1s;
}
.fade-leave-active {
  transform: translateX(0px);
  opacity: 1;
  transition: all 1s;
}
.fade-enter {
  opacity: 0;
  transform: translateX(200px);
}
.fade-leave-to {
  opacity: 0;
  transform: translateX(-200px);
}
</style>
