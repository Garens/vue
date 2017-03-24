<template>
  <g>
    <polygon :points="points"></polygon>    <!-- 此处根据points生成对应的polygon -->
    <circle cx="100" cy="100" r="80"></circle>
    <!-- 此处也会根据数组stats循环输出axis-label组件，axis-label也是自定义组件 -->
    <axis-label
      v-for="(stat, index) in stats"
      :stat="stat"
      :index="index"
      :total="stats.length">
    </axis-label>
  </g>
</template>

<script>
  export default {
    props: ['stats'],   //相当于初始化组件的一个参数stats，作为形参存在
    computed: {
      // 此处根据形参stats生成对应参数points
      points: function () {
        var total = this.stats.length
        return this.stats.map(function (stat, i) {
          var point = valueToPoint(stat.value, i, total)
          return point.x + ',' + point.y
        }).join(' ')
      }
    },
    components: {
      // axis-label组件，此处也可以写成Polygraph组件，但结构简单，可直接用template方式进行编写
      'axis-label': {
        props: {
          stat: Object,
          index: Number,
          total: Number
        },
        template: '<text :x="point.x" :y="point.y">{{stat.label}}</text>',
        computed: {
          point: function () {
            return valueToPoint(
              +this.stat.value + 10,
              this.index,
              this.total
            )
          }
        }
      }
    }
  }

  // 转换函数
  function valueToPoint (value, index, total) {
    var x     = 0
    var y     = -value * 0.8
    var angle = Math.PI * 2 / total * index
    var cos   = Math.cos(angle)
    var sin   = Math.sin(angle)
    var tx    = x * cos - y * sin + 100
    var ty    = x * sin + y * cos + 100
    return {
      x: tx,
      y: ty
    }
  }
</script>

<style scoped>
  body {
    font-family: Helvetica Neue, Arial, sans-serif;
  }

  polygon {
      fill: #42b983;
      opacity: .75;
  }

  circle {
      fill: transparent;
      stroke: #999;
  }

  text {
      font-family: Helvetica Neue, Arial, sans-serif;
      font-size: 10px;
      fill: #666;
  }

  label {
      display: inline-block;
      margin-left: 10px;
      width: 20px;
  }
</style>
