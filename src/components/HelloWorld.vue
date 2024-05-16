<template>
  <div id="main" ref="chartContainer" style="width: 200px; height: 200px"></div>
</template>

<script>
import * as echarts from 'echarts'

export default {
  mounted() {
    this.initChart()
  },
  methods: {
    initChart() {
      const chartContainer = this.$refs.chartContainer
      if (!chartContainer) {
        console.error('Chart container not found')
        return
      }

      const chart = echarts.init(chartContainer)
      const value = 75 // 进度条的值

      const option = {
        series: [
          {
            type: 'pie',
            radius: ['70%', '90%'], // 设置环形大小
            avoidLabelOverlap: false,
            label: {
              show: false,
            },
            data: [
              {
                value: value, // 进度条值
                itemStyle: {
                  color: '#67e0e3', // 进度条颜色
                },
              },
              {
                value: 100 - value, // 剩余部分值
                itemStyle: {
                  color: '#f0f0f0', // 剩余部分颜色
                },
              },
            ],
          },
        ],
        graphic: [
          {
            type: 'text',
            left: 'center',
            top: 'center',
            style: {
              text: value, // 中间数值
              textAlign: 'center',
              fontSize: 30,
              fontWeight: 'bold',
              fill: '#333', // 中间数值颜色
            },
          },
        ],
      }
      chart.setOption(option)
    },
  },
}
</script>

<style scoped>
#main {
  width: 100%;
  height: 100%;
}
</style>
