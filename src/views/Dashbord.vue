<template>
  <div>
<!--    <el-row :gutter="10" style="margin-bottom: 60px;border:1px solid red">-->
<!--      <el-col :span="6">-->
<!--        <el-card style="color: #409EFF">-->
<!--          <div><i class="el-icon-user-solid" /> 用户总数</div>-->
<!--          <div style="padding: 10px 0; text-align: center; font-weight: bold">-->
<!--            100-->
<!--          </div>-->
<!--        </el-card>-->
<!--      </el-col>-->
<!--      <el-col :span="6">-->
<!--        <el-card style="color: #F56C6C">-->
<!--          <div><i class="el-icon-money" /> 销售总量</div>-->
<!--          <div style="padding: 10px 0; text-align: center; font-weight: bold">-->
<!--            ￥ 1000000-->
<!--          </div>-->
<!--        </el-card>-->
<!--      </el-col>-->
<!--      <el-col :span="6">-->
<!--        <el-card style="color: #67C23A">-->
<!--          <div><i class="el-icon-bank-card" /> 收益总额</div>-->
<!--          <div style="padding: 10px 0; text-align: center; font-weight: bold">-->
<!--            ￥ 300000-->
<!--          </div>-->
<!--        </el-card>-->
<!--      </el-col>-->
<!--      <el-col :span="6">-->
<!--        <el-card style="color: #E6A23C">-->
<!--          <div><i class="el-icon-s-shop" /> 门店总数</div>-->
<!--          <div style="padding: 10px 0; text-align: center; font-weight: bold">-->
<!--            20-->
<!--          </div>-->
<!--        </el-card>-->
<!--      </el-col>-->
<!--    </el-row>-->
    <el-row >
      <el-col :span="12">
        <div id="main" style="width: 500px; height: 450px"></div>
      </el-col>

      <el-col :span="12" >
        <div id="pie" style="width: 500px; height: 400px" ></div>
      </el-col>
    </el-row>

    <el-row style="margin-top: 20px">
      <el-col :span="12">
        <div id="candlestick" style="width: 500px; height: 450px"></div>
      </el-col>
    </el-row>

  </div>
</template>

<script>
import * as echarts from 'echarts'

export default {
  name: "Home",
  data() {
    return {
      disable:false

    }
  },
  mounted() {  // 页面元素渲染之后再触发
    const option = {
      title: {
        text: '各季度会员数量统计',
        subtext: '趋势图',
        left: 'center'
      },
      tooltip: {
        trigger: 'item'
      },
      legend: {
        orient: 'vertical',
        left: 'left'
      },
      xAxis: {
        type: 'category',
        data: ["第一季度", "第二季度", "第三季度", "第四季度"]
      },
      yAxis: {
        type: 'value'
      },
      series: [
        {
          name: "星巴克",
          data: [],
          type: 'bar'
        },
        {
          name: "星巴克",
          data: [],
          type: 'line'
        },
        {
          name: "瑞幸咖啡",
          data: [],
          type: 'bar'
        },
        {
          name: "瑞幸咖啡",
          data: [],
          type: 'line'
        }
      ]
    };

    // 饼图

    const pieOption = {
      title: {
        text: '各季度会员数量统计',
        subtext: '比例图',
        left: 'center'
      },
      tooltip: {
        trigger: 'item',
        formatter: '{a} <br/>{b} : {c} ({d}%)'
      },
      legend: {
        orient: 'vertical',
        left: 'left'
      },
      series: [
        {
          name: "瑞幸咖啡",
          type: 'pie',
          radius: '50%',
          center: ['75%', '50%'],
          label:{            //饼图图形上的文本标签
            normal:{
              show:true,
              position:'inner', //标签的位置
              textStyle : {
                fontWeight : 300 ,
                fontSize : 14,    //文字的字体大小
                color: "#fff"
              },
              formatter:'{d}%'
            }
          },
          data: [
            {name: "第一季度", value: 5},
            {name: "第二季度", value: 6},
            {name: "第三季度", value: 7},
            {name: "第四季度", value: 8}
            ],  // 填空
          emphasis: {
            itemStyle: {
              shadowBlur: 10,
              shadowOffsetX: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        }
      ],

    };

    const candlestickOption = {
      title: {text: 'XX股票'},
      xAxis: {
        data: ['2022-1', '2022-2', '2022-3', '2022-4'],
      },
      yAxis: {
        scale: true,
        splitArea: {
          show: true
        }
      },
      legend: {
        bottom: 10,
        left: 'center',
        data: ['Dow-Jones index', 'MA5', 'MA10', 'MA20', 'MA30']
      },
      tooltip: {
        trigger: 'axis',
        axisPointer: {
          type: 'cross'
        }
      },
      dataZoom: {
        show: true,
        type: 'slider',
        startValue: 0,
        endValue: 100
      },
      series: [
        {
          type: 'candlestick',
          data: [
              [22, 23, 21, 19],
              [19, 21, 29, 31],
              [28, 18, 31, 20],
              [30, 27, 38, 18],
          ]
        }
      ]
    }


    var chartDom = document.getElementById('main');
    var myChart = echarts.init(chartDom);

    var pieDom = document.getElementById('pie');
    var pieChart = echarts.init(pieDom);

    var candlestickDom = document.getElementById('candlestick');
    var candlestickChart = echarts.init(candlestickDom);
    candlestickChart.setOption(candlestickOption)

    this.request.get("/echarts/members").then(res => {
      // 填空
      // option.xAxis.data = res.data.x
      option.series[0].data = res.data
      option.series[1].data = res.data

      option.series[2].data = [5,6,7,8]
      option.series[3].data = [5,6,7,8]
      // 数据准备完毕之后再set
      myChart.setOption(option);


      pieOption.series[0].data = [
        {name: "第一季", value: res.data[0]},
        {name: "第二季度", value: res.data[0]},
        {name: "第三季度", value: res.data[2]},
        {name: "第四季度", value: res.data[3]},
      ]
      pieChart.setOption(pieOption)
    })
  }
}
</script>

<style scoped>

</style>
