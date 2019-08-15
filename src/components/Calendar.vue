<template>
  <div class="wrap">
    <div
      v-for="item in months"
      :key="item.month"
      class="cal-wrap"
    >
      <div class="cal-title">{{ item.year + '年' + item.month + '月' }}</div>
      <span
        v-for="week in weeks"
        :key="week"
      >{{ week }}</span>
      <span
        v-for="{b,index} in item.beginWeekday"
        :key="index"
      >
            </span>
      <span
        v-for="day in item.days"
        :key="day.index"
        :class="day.checked ? 'active' : 'day'"
        @click="clickHandler(day)"
      ><a href="javascript:;">{{ day.index }}</a>
            </span>
    </div>
  </div>
</template>
<script>
  export default {
    name: "Calendar",
    props: {
      checked:
        {
          type: Array,
          require: true
        },
      weeks:
        {
          type: Array,
          default: () => ["日", "一", "二", "三", "四", "五", "六"],
        },
      clickHandler: {
        type: Function,
        default: (value)=>{
          console.log("默认点击处理事件");
          console.log(value);
        }
      }
    },
    data() {
      return {
        months: []
      };
    },
    created() {
      this.initCalendar()
    },
    watch: {
      checked(val) {
        // 监听已选中数组变化，重新渲染日历
        this.initCalendar();
      }
    },
    methods:
      {
        isLeapYear(year) {
          return year % 4 === 0 && year % 100 !== 0 || year % 400 === 0;
        },
        getDayByMonth(year, month) {
          let days;
          switch (month) {
            case 2:
              days = this.isLeapYear() ? 29 : 28;
              break;
            case 4:
            case 6:
            case 9:
            case 11:
              days = 30;
              break;
            default:
              days = 31;
          }
          return days;
        },
        getWeekday(year, month) {
          let days = 1;
          for (let i = 1; i < month; i++) {
            days += this.getDayByMonth(year, i);
          }
          for (let i = 1900; i < year; i++) {
            days += this.isLeapYear(i) ? 366 : 365;
          }
          let weeks = days % 7;
          return weeks;
        },
        initCalendar() {
          let time = new Date();
          let year = time.getFullYear();
          this.months = [];
          for (let i = 1; i <= 12; i++) {
            let totalDay = this.getDayByMonth(year, i);
            let days = [];
            for (let j = 1; j <= totalDay; j++) {
              // 构建日期,检查选中项
              let dayObj = {
                index: j,
                value: year + "" + (i < 10 ? "0" + i : i) + "" + (j < 10 ? "0" + j : j),
                checked: this.checked.includes(year + "" + (i < 10 ? "0" + i : i) + "" + (j < 10 ? "0" + j : j))
              };
              days.push(dayObj);
            }
            let monthObj = {
              year: year,
              month: i,
              beginWeekday: this.getWeekday(year, i),
              days: days
            };
            this.months.push(monthObj);
          }
        }
      }
  };
</script>
<style lang="scss" scoped>
  a{
    color: #000;
    text-decoration: none;
  }
  .wrap {
    margin-left: 20px;
    margin-top: 20px;

    .switch {
      padding: 10px 5px;
    }
  }

  .cal-wrap {
    width: 282px;
    background: #fff;
    display: flex;
    flex-wrap: wrap;
    float: left;
    border: 1px solid rgb(235, 235, 235);
    height: 310px;

    span {
      width: 28px;
      height: 28px;
      margin: 6px;
      line-height: 28px;
      text-align: center;
      color: #000;
      font-size: 12px;
      transition: all 0.3s;
    }

    .day {
      border-radius: 50%;

      &:hover {
        a{
          color: #fff;
        }
        background-color: #409eff;
      }
    }

    .active {
      a{
        color: #fff;
      }
      border-radius: 50%;
      background-color: #409eff;
    }

    .week {
      color: #000;
    }
  }

  .cal-title {
    padding-top: 10px;
    height: 24px;
    font-size: 16px;
    line-height: 24px;
    display: flex;
    padding-left: 35%;
    width: 100%;
    text-align: center;
  }
</style>

