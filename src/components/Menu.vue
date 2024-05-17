<template>
  <div class="menu-wrapper">
    <van-popup v-model:show="show" class="popup_style">
      <h1>自定义菜单</h1>
      <p class="tip">- 菜名间要以空格区分 -</p>
      <div class="textarea-wrapper"><textarea v-model="menuStr"></textarea></div>
      <van-button round type="primary" size="large" @click="handleSubmit">确定</van-button>
    </van-popup>
  </div>
</template>

<script setup>
import { defineProps } from 'vue'
import { ref, defineEmits } from 'vue'

let props = defineProps({
  menuList: Array,
})

const menuStr = ref('')
const show = ref(true)
menuStr.value = props.menuList.join(' ')

let emit = defineEmits(["click"])

function handleSubmit() {
  const str = menuStr.value.trim()
  if (str === '') {
    alert('你怕是要饿死自己！')
  } else {
    show.value = !show.value
    let arr = str.split(' ')
    setTimeout(() => {
      emit('updateMenu', arr)
      show.value = !show.value
    }, 500);
  }
}

</script>


<style lang="scss" scoped>
.popup_style {
  padding: 15px 20px;
  width: 80%;
  ;

  h1,
  p {
    text-align: center;
    font-size: 20px;
    color: #333;
  }

  .tip {
    margin: 10px 0;
    font-size: 16px;
  }

  .textarea-wrapper {
    height: 300px;
    padding: 8px;
    border: 1px solid black;
    border-radius: 4px;

    textarea {
      width: calc(100%);
      height: 100%;
      resize: none;
      font-size: 16px;
      line-height: 20px;
      border: none;
    }
  }

  .van-button {
    margin: 12px auto;
    font-size: 16px;
    color: #fff;
  }
}
</style>
