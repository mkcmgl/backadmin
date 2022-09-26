<template>
  <div class="charts" ref="charts"></div>
</template>

<script>
import echarts from "echarts";
export default {
  name: "progressCharts",

  data() {
    return {};
  },

  mounted() {
    let progressCharts = echarts.init(this.$refs.charts);
    progressCharts.setOption({
      xAxis: {
        show: false,
        min: 0,
        max: 100,
      },
      yAxis: {
        show: false,
        type: "category",
      },

      series: [
        {
          type: "bar",
          data: [78],
          color: "cyan",
          barWidth: 10,
          color: "yellowgreen",
          showBackground: true,
          backgroundStyle: {
            color: "#eee",
          },
          //文本标签
          label: {
            show: true,
            //改变文本内容
            formatter: "|",
            //文本标签位置调试
            position: "right",
          },
        },
      ],
      grid: {
        left: 0,
        top: 0,
        right: 0,
        bottom: 0,
      },
      tooltip: {},
    });
  },

  methods: {},
};
</script>

<style  scoped>
.charts {
  width: 100%;
  height: 100%;
}
</style>