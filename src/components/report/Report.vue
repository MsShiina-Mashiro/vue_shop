<template>
  <div>
    <!-- 面包屑管理区域 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>数据统计</el-breadcrumb-item>
      <el-breadcrumb-item>数据报表</el-breadcrumb-item>
    </el-breadcrumb>
    <!-- 卡片视图区域 -->
    <el-card>
      <!-- 2、为 ECharts 初始化一个具备初始宽高的DOM -->
      <div id="main" style="width: 750px; height: 400px;"></div>
    </el-card>
  </div>
</template>

<script>
// 1、导入 Echarts
import echarts from "echarts";
import _ from "lodash"

export default {
  data() {
    return {
      // 需要合并的对象
      options: {
        title: {
          text: "用户来源"
        },
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "cross",
            label: {
              backgroundColor: "#E9EEF3"
            }
          }
        },
        grid: {
          left: "3%",
          right: "4%",
          bottom: "3%",
          containLabel: true
        },
        xAxis: [
          {
            boundaryGap: false
          }
        ],
        yAxis: [
          {
            type: "value"
          }
        ]
      }
    };
  },
  created() {},
  methods: {},
  // 此视页面上的元素已经被渲染完毕了
  async mounted() {
    // 3、基于准备好的 Dom 初始化 echarts 实例
    var myChart = echarts.init(document.getElementById("main"));

    const { data: res } = await this.$http.get("reports/type/1");
    if (res.meta.status !== 200) {
      return this.$message.error("获取折线图数据失败！");
    }

    // 4、准备数据和配置项
    const result = _.merge(res.data, this.options)
    // 5、展示数据
    myChart.setOption(result);
  }
};
</script>

<style lang="less" scoped>
</style>