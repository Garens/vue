<template>
  <main-layout>
    <!-- demo root element -->
    <div id="demo">
      <pre id="raw">{{ stats }}</pre>
      <!-- Use the component -->
      <svg width="200" height="200">
        <polygraph :stats="stats"></polygraph>    <!-- 此处为调用组件，并绑定对应数据 -->
      </svg>
      <!-- controls -->
      <div v-for="stat in stats">
        <label>{{stat.label}}</label>
        <input type="range" v-model="stat.value" min="0" max="100">
        <span>{{stat.value}}</span>
        <button @click="remove(stat)" class="remove">X</button>
      </div>
      <form id="add">
        <input name="newlabel" v-model="newLabel">
        <button @click="add">Add a Stat</button>
      </form>
    </div>
  </main-layout>
</template>

<script>
  import MainLayout from '../layouts/Main.vue'
  import Polygraph from '../components/Polygraph.vue'

// 此处为调用组件使用的数据，所以要放在调用组件的页面
var stats = [
  { label: 'A', value: 100 },
  { label: 'B', value: 100 },
  { label: 'C', value: 100 },
  { label: 'D', value: 100 },
  { label: 'E', value: 100 },
  { label: 'F', value: 100 }
]
  export default {
    data: function(){
      return {
        newLabel: '',
        stats:stats   //把数据进行初始化
      }
    },
    methods: {  //添加及删除的方法
      add: function (e) {
        e.preventDefault()
        if (!this.newLabel) return
        this.stats.push({
          label: this.newLabel,
          value: 100
        })
        this.newLabel = ''
      },
      remove: function (stat) {
        if (this.stats.length > 3) {
          this.stats.splice(this.stats.indexOf(stat), 1)
        } else {
          alert('Can\'t delete more!')
        }
      }
    },
    components: {   //组件列表，调用组件
      MainLayout,Polygraph
    }
  }
</script>

<style scoped>
  #raw {
      float: right;
      width: 100px;
      height: auto;
  }
</style>
