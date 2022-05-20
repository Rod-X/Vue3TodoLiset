<template>
  <div>
    <input type="text" v-model="val">
    <button @click="addTodo">添加</button>
  </div>
  <div v-if="changeDemo.show">
    <span>原活动：{{ changeDemo.name }}</span>
    <span>改为</span>
    <input type="text" v-model="changeDemo.val">
    <button @click="saveChange">保存</button>
  </div>
  <ul>
    <li v-for="(todo, index) in todos" :key="todo.name">
      <input type="checkbox" v-model="todo.done">
      <span>{{ todo.name }}</span>
      <span @click="deltodo(index)" style="color:red;margin-left:10px">删除</span>
      <span @click="changeTode(index)" style="color:green;margin-left:10px">改</span>
    </li>
  </ul>
  <div>
    <input type="checkbox" v-model="isAll">
    <span>全选</span>
    <span>{{ selNum }}/{{ len }}</span>
    <button @click="delAll">删除</button>

  </div>
</template>

<script lang="ts" setup>
import { computed, ref } from "vue";
interface todo {
  done: boolean,
  name: string
}
let val = ref('')
let todos = ref<todo[]>([])
// 增
const addTodo=function() {
  if (val.value) {
    todos.value.push({
      done: false,
      name: val.value
    })
    val.value = ''
  }
}
// 删
function deltodo(index: number) {
  todos.value.splice(index, 1)
}
// 全选功能实现
let len = computed<number>(() => todos.value.length)
let selNum = computed<number>(() => todos.value.filter(v => v.done).length)
let isAll = computed<boolean>({
  get() {
    return len.value === 0 ? false : len.value == selNum.value
  },
  set(val: boolean) {
    todos.value.forEach(v => { v.done = val })
  }
})
let changeDemo = ref({
  show: false,
  name: '',
  val: '',
  changeIndex: -1
})
// 改
function changeTode(i: number) {
  const demo = todos.value[i]
  changeDemo.value = {
    show: true,
    name: demo.name,
    val: '',
    changeIndex: i
  }
}
function saveChange() {
  let { changeIndex, val } = changeDemo.value
  todos.value[changeIndex].name = val
  changeDemo.value = {
    show: false,
    name: '',
    val: '',
    changeIndex: -1
  }
}
// 全选删除
function delAll() {
  if (isAll.value) {
    todos.value = []
  }
}
</script>