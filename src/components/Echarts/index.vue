<!--
 * @Description: echarts组件
 * @Version:
 * @Autor:
 * @Date: 2020-02-24 17:19:35
 * @LastEditTime: 2020-03-02 11:40:24
 -->
<template>
  <div>
    <div :id="idSelect" :style="{ width: '100%', height: height + 'px' }"></div>
  </div>
</template>

<script>
import echarts from 'echarts'
import { on, off } from '@/utils/utils'
import china from './map/china'
export default {
  props: {
    // 移入移除事件flag
    mouseoverFlag: {
      type: Boolean,
      default: false
    },
    height: {
      type: Number,
      default: 100
    },
    // echarts选项
    option: {
      default: {}
    },
    // 这是需要初始化的id,保证每个都不同
    idSelect: {
      default: ''
    }
  },
  data() {
    return {
      // echart实例
      myChart: null
    }
  },
  watch: {
    option: {
      handler: function(val) {
        if (this.myChart) {
          // this.myChart.clear()
          this.myChart.setOption(val)
        }
      },
      deep: true
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.drawLine()
      // 移入移除事件
      if (this.myChart && this.mouseoverFlag) {
        this.myChart.on('mouseover', data => {
          this.$emit('mouseover', data)
        })
      }
      if (this.myChart) {
        this.myChart.on('click', data => {
          this.$emit('click', data)
        })
      }
    })
  },
  beforeDestroy() {
    off(window, 'resize', this.resize)
  },
  methods: {
    reset() {
      this.myChart.clear()
      this.myChart.setOption(this.option)
    },
    resize() {
      this.myChart.resize()
    },
    drawLine() {
      console.log(123)
      // 基于准备好的dom，初始化echarts实例
      let ID = this.idSelect
      this.myChart = echarts.init(document.getElementById(ID))
      this.myChart.setOption(this.option)
      on(window, 'resize', this.resize)
    }
  }
}
</script>

<style scoped></style>
