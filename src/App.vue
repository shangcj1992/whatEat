<!-- 
 * @description: 
 * @fileName: App.vue
 * @author:  shangchangjun
 * @date:  2024/05/17 09:31:55
 * @version: V1.0.0 
!-->
<script setup>
import Ball from './components/Ball.vue';
import Menu from './components/Menu.vue';
import { ref, onMounted, computed } from 'vue';
const mealArr = ref(["别吃别吃", "驴肉火烧", "馄饨", "烩面", "热干面", "刀削面", "油泼面", "炸酱面", "炒面", "臊子面", "重庆小面", "米线", "酸辣粉", "土豆粉", "螺狮粉", "凉皮儿", "麻辣烫", "肉夹馍", "羊肉泡馍", "炒饭", "盖浇饭", "烤肉饭", "黄焖鸡米饭", "麻辣香锅", "小火锅", "酸菜鱼", "烤串", "披萨", "烤鸭", "汉堡", "炸鸡", "寿司", "煎饼果子", "南瓜粥", "小龙虾", "牛排", "砂锅", "大排档", "西餐", "自助餐", "小笼包", "水果", "西北风", "烧烤", "泡面", "水饺", "日本料理", "涮羊肉", "兰州拉面", "面包", "小笼包", "沙县小吃", "烤鱼", "海鲜", "铁板烧", "韩国料理", "甜点", "鸭血粉丝汤"]);
const ballArr = ref([]);//背景中随机变化的字符集。同步mealArr
const mealName = ref('');//随机后选中的字符串
const timerAdd = ref(null)
const timerDel = ref(null)
const timerClean = ref(null);//记录计时器实例
const clientHeight = ref(0)
const clientWidth = ref(0)
const menuShow = ref(false)
const btnName = computed(() => {
  return timerAdd.value === null ? '开始' : '停止'
})
onMounted(() => {
  const dom = document.getElementsByClassName('container')[0]
  clientWidth.value = dom.clientWidth
  clientHeight.value = dom.clientHeight
})

function handleClick() {
  timerAdd.value ? stop() : start()
}

/**
 * 停止
 */
const stop = () => {
  clearInterval(timerAdd.value)
  timerAdd.value = null
  timerClean.value = setTimeout(() => {
    ballArr.value = []
    clearInterval(timerDel.value)
  }, 3000)
}


/**
 * 开始
 */
const start = () => {
  clearTimeout(timerClean.value)
  timerAdd.value = setInterval(() => {
    const len = mealArr.value.length;
    mealName.value = mealArr.value[parseInt(Math.random() * len)];
    ballArr.value.push({
      top: parseInt(Math.random() * clientHeight.value) + 'px',
      left: parseInt(Math.random() * clientWidth.value) + 'px',
      name: mealArr.value[parseInt(Math.random() * len)],
      fontSize: 12 + parseInt(Math.random() * 20) + 'px',
    })
  }, 100)

  timerDel.value = setInterval(() => {
    ballArr.value.shift()
  }, 3000)

  setTimeout(() => {
    stop()
  }, 20000)
}

function toggle() {
  menuShow.value = !menuShow.value
}
function updateMenu(menuArr) {
  mealArr.value = menuArr
  toggle()
}
</script>
<template>
  <div class="container">
    <img class="title" src="./assets/img/logo.png" alt="">
    <Ball v-for="ball in ballArr" :key="ball.name" :ball="ball" />
    <div class="_content">
      <span class="name" v-show="mealName">{{ mealName }}</span>
      <van-button round :type="timerAdd == null ? 'success' : 'danger'" size="large" @click="handleClick()">{{ btnName
        }}</van-button>
      <van-button round type="primary" size="large" v-show="!timerAdd" @click="toggle">编辑菜单</van-button>
    </div>
    <Menu v-show="menuShow" :menu-list="mealArr" @updateMenu="updateMenu"></Menu>
  </div>
</template>
<style lang='scss' scoped>
.container {
  position: relative;
  width: 100vw;
  height: 100vh;
  background-image: url("./assets/img/bg.jpg");
  overflow: hidden;

  ._content {
    z-index: 9;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    height: 100%;
  }

  .title {
    position: absolute;
    top: 50px;
    left: 50%;
    transform: translateX(-50%);
    width: 270px;
  }

  .name {
    font-size: 35px;
    color: #ff9732;
    font-weight: 800;
  }
}

.van-button {
  width: 200px;
  margin-top: 20px;
  font-size: 20px;
  transition: background-color 300ms;
  cursor: pointer;
}

.btn:hover {
  background-color: rgba(0, 0, 0, .4);
}
</style>