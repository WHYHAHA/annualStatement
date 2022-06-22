<template lang="">
  <div class="bigBox">
    <a-card title="年度报告" :bordered="false" style="width: 80%;margin: 0 auto;">
      <!-- 数据统计 -->
      <div class="statistics">
        <div class="statistics_box">
          <div class="title">工作事件</div>
          <h3>{{yearData.length}}件</h3>
        </div>
        <div class="statistics_box">
          <div class="title">热力值</div>
          <h3>5678</h3>
        </div>
      </div>

      <!-- 点状列表 -->
      <div class="yearline" ref="yearline">
        <a-tooltip v-for="(item, index) in yearData" :key="index" style="position: relative;">
          <template slot="title">
            <div>{{ item.time}}</div>
            <div>
              {{ item.desc }}
            </div>
          </template>
          <!-- 左侧星期简写 -->
          <div class="weekHint" v-if="index==1">Mon</div>
          <div class="weekHint" v-if="index==3">Wed</div>
          <div class="weekHint" v-if="index==5">Fri</div>
          <!-- 上方月份简写 -->
          <div class="monthHint" v-if="item.showMonth">{{monthHintVal(item.month)}}</div>
          <div :class="item.level+' box'"></div>
        </a-tooltip>
        <div class="yearline_bottom">
          less
          <span class="box green1" style="margin-bottom: 0;margin-left: 5px;"></span>
          <span class="box green2" style="margin-bottom: 0;"></span>
          <span class="box green3" style="margin-bottom: 0;"></span>
          <span class="box green4" style="margin-bottom: 0;"></span>
          <span class="box green5" style="margin-bottom: 0;"></span>
          more
        </div>
        <!-- 操作btn -->
        <div class="btnBox">
          <a-space>
            <a-button type="primary" icon="download" size="small" style="font-size: 12px;">
              导出考勤表
            </a-button>
            <a-button type="primary" icon="download" size="small" style="font-size: 12px;">
              导出事件表
            </a-button>
          </a-space>
        </div>
      </div>

    </a-card>
  </div>
</template>
<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        yearData: [],
      }
    },
    computed: {
      monthHintVal() {
        return function (value) {
          switch (value) {
            case '01':
              return "Jun"
              break;

            case '02':
              return "Feb"
              break;

            case '03':
              return "Mar"
              break;

            case '04':
              return "Apr"
              break;

            case '05':
              return "May"
              break;

            case '06':
              return "Jun"
              break;

            case '07':
              return "Jul"
              break;

            case '08':
              return "Aug"
              break;
            case '09':
              return "Sept"
              break;

            case '010':
              return "Oct"
              break;

            case 11:
              return "Nov"
              break;

            case 12:
              return "Dec"
              break;
            default:
              break;
          }
        }
      }
    },
    mounted() {
      this.yearDataInit();
    },
    methods: {
      //初始化时间数组
      yearDataInit() {
        let newTime = new Date().toLocaleDateString().replace(/\//g, '-')
        let oldTime = new Date().getTime() - 365 * 24 * 60 * 60 * 1000
        oldTime = new Date(oldTime).toLocaleDateString().replace(/\//g, '-')
        this.yearData = this.getDiffDate('2020-01-01', "2020-12-32")

        // 设置showMonth
        // 根据能被7取余的头部位置，判断之后六个是否有包含月份第一天的，设置显示月份
        this.yearData.forEach((element, index) => {
          if (index % 7 == 0 && (index + 6) < this.yearData.length) {
            for (let flag = index; flag <= (index + 6); flag++) {
              this.yearData[flag].day == '01' ? element.showMonth = true : '';
              this.yearData[flag].day == '01' ? element.month = this.yearData[flag].month : '';
            }
          }
        });

        console.log(newTime);
        console.log(this.yearData);
      },
      //均衡产生随机数
      RandomNumBoth(Min, Max) {
        var Range = Max - Min;
        var Rand = Math.random();
        var num = Min + Math.round(Rand * Range); //四舍五入
        return num;
      },
      getDiffDate(start, end) {
        var startTime = this.getDate(start);
        var endTime = this.getDate(end);
        var dateArr = [];

        while ((endTime.getTime() - startTime.getTime()) > 0) {
          var year = startTime.getFullYear();
          var month = startTime.getMonth().toString().length === 1 ? "0" + (parseInt(startTime.getMonth().toString(), 10) + 1) : (startTime.getMonth() + 1);
          var day = startTime.getDate().toString().length === 1 ? "0" + startTime.getDate() : startTime.getDate();
          let randomNum = this.RandomNumBoth(1, 5);
          dateArr.push({
            time: year + "-" + month + "-" + day,
            desc: randomNum == 1 ? '暂无完成' : '完成今日任务xxxxxx',
            level: 'green' + randomNum,
            month: month,
            originalMonth: month,
            day: day
          });
          startTime.setDate(startTime.getDate() + 1);

        }

        return dateArr;

      },
      getDate(datestr) {
        var temp = datestr.split("-");
        if (temp[1] === '01') {
          temp[0] = parseInt(temp[0], 10) - 1;
          temp[1] = '12';
        } else {
          temp[1] = parseInt(temp[1], 10) - 1;
        }

        //new Date()的月份入参实际都是当前值-1

        var date = new Date(temp[0], temp[1], temp[2]);
        return date;

      },
    },
  }
</script>
<style lang="" scoped>
  .bigBox {
    padding-top: 60px;
  }

  .statistics {
    display: flex;
    align-items: center;
    padding-left: 15%;
  }

  .statistics_box {
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: flex-end;
    padding: 15px 20px;
    text-align: right;
  }

  .statistics_box:nth-of-type(1) {
    padding-left: 0;
  }

  .statistics_box:nth-of-type(1)::after {
    position: absolute;
    top: 27px;
    right: 0;
    width: 1px;
    height: 40px;
    background-color: #e8e8e8;
    content: "";
  }

  .statistics_box .title {
    color: rgba(0, 0, 0, .45);
    font-size: 20px;
  }

  .yearline {
    position: relative;
    display: flex;
    flex-direction: column;
    flex-wrap: wrap;
    align-content: space-evenly;
    height: 105px;
    width: 70%;
    margin: 0 auto;
    margin-top: 20px;
    margin-bottom: 80px;
  }

  .yearline .box {
    width: 10px;
    height: 10px;
    border-radius: 4px;
    margin-right: 5px;
    margin-bottom: 5px;
  }

  .yearline_bottom {
    display: flex;
    align-items: center;
    justify-content: space-between;
    position: absolute;
    bottom: -20px;
    right: 0;
  }

  .btnBox {
    position: absolute;
    left: 0;
    bottom: -40px;
    display: flex;
    justify-content: flex-start;
    margin-top: 20px;

  }

  .green1 {
    background: #E4E7ED;
  }

  .green2 {
    background: #B2FC5C;
  }

  .green3 {
    background: #75FB4C;
  }

  .green4 {
    background: #5DC93B;
  }

  .green5 {
    background: #43972A;
  }

  .weekHint {
    position: absolute;
    left: -30px;
    /* top为box的margin-bottom值 */
    top: -5px;
    font-size: 12px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }

  .monthHint {
    position: absolute;
    left: 0px;
    /* top为box的margin-bottom值 */
    top: -20px;
    font-size: 12px;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }

</style>
