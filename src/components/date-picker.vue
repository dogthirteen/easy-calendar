<template>
  <div class="cc-calendar">
    <calendarHeader :headOptions="headOptions" @handlePrevMonth='handlePrevMonth' @handleNextMonth='handleNextMonth' @handleToday='handleToday' />
    <ul class="calendar-week ">
      <li v-for="(item, index) in calendarTitleArr" :key="index" class="week-item">{{item}}</li>
    </ul>
    <ul class="calendar-view">
      <li v-for="(item, index) in visibleCalendar" :key="index" class="date-view" :class="[
          {todayBg:checkDate === `${item.year}年${item.month}月${item.day}日`},
        ]" @click="handleClickDay(item)">
        <span class="date-day" :style="dayStyle" :class="[{'opacity-class': !isCurrentMonth(item.date)}]">
          {{item.day}}
        </span>
        <span class="calendar-num">
          {{item.calendarNum}}
        </span>
      </li>
    </ul>
  </div>
</template>

<script>
import calendarHeader from './calendar-head.vue';
import calendar from '@/utils/calendar.js';

export default {
  name: 'cc-calendar',
  componentName: 'cc-calendar',
  props: {
    options: Object
  },
  components: {
    calendarHeader
  },
  data() {
    let { year, month, day } = calendar.getNewDate(new Date());
    return {
      headOptions: {
        style: this.options.headStyle,
        date: '',
      },
      checkDate: '',
      calendarTitleArr: [
        '星期一',
        '星期二',
        '星期三',
        '星期四',
        '星期五',
        '星期六',
        '星期日 '
      ],
      time: { year, month, day },
      calendarList: []
    }
  },
  computed: {
    dayStyle: function () {
      return {
        textAlign: this.options.viewStyle.day,
      }
    },
    visibleCalendar() {
      let calendatArr = [];
      let { year, month, day } = calendar.getNewDate(calendar.getDate(this.time.year, this.time.month, 1));
      let currentFirstDay = calendar.getDate(year, month, 1);

      // 获取当前月第一天星期几
      let weekDay = currentFirstDay.getDay();
      let startTime = null;

      if (weekDay == 0) {
        startTime = currentFirstDay - 6 * 24 * 60 * 60 * 1000;
      } else {
        startTime = currentFirstDay - (weekDay - 1) * 24 * 60 * 60 * 1000;
      }

      let monthDayNum;
      if (weekDay == 5 || weekDay == 6 || weekDay == 0) {
        monthDayNum = 42
      } else {
        monthDayNum = 35
      };

      for (let i = 0; i < monthDayNum; i++) {
        calendatArr.push({
          date: new Date(startTime + i * 24 * 60 * 60 * 1000),
          year: new Date(startTime + i * 24 * 60 * 60 * 1000).getFullYear(),
          month: new Date(startTime + i * 24 * 60 * 60 * 1000).getMonth() + 1,
          day: new Date(startTime + i * 24 * 60 * 60 * 1000).getDate(),
          clickDay: false,
        })
      };

      this.headOptions.date = `${year}年${calendar.formatDate(Number(month) + 1)}月`;

      return calendatArr
    }
  },
  methods: {
    // 是否是当前月
    isCurrentMonth(date) {
      let { year: currentYear, month: currentMonth } = calendar.getNewDate(calendar.getDate(this.time.year, this.time.month, 1));
      let { year, month, day } = calendar.getNewDate(date);
      return currentYear == year && currentMonth == month
    },
    // 上一个月
    handlePrevMonth() {
      let prevMonth = calendar.getDate(this.time.year, this.time.month, 1);
      prevMonth.setMonth(Number(prevMonth.getMonth()) - 1);
      this.time = calendar.getNewDate(prevMonth);
      this.headOptions.date = `${calendar.englishMonth(this.time.month)} ${this.time.year}`;
      this.$emit('handlePrevMonth', new Date(prevMonth));
    },
    // 下一个月
    handleNextMonth() {
      let nextMonth = calendar.getDate(this.time.year, this.time.month, 1);
      nextMonth.setMonth(Number(nextMonth.getMonth()) + 1);
      this.time = calendar.getNewDate(nextMonth);
      this.headOptions.date = `${calendar.englishMonth(this.time.month)} ${this.time.year}`;
      this.$emit('handleNextMonth', new Date(nextMonth));
    },
    // 点击回到今天
    handleToday() {
      this.time = calendar.getNewDate(new Date());
      let { year: currentYear, month: currentMonth, day: currentDay } = calendar.getNewDate(new Date());
      currentMonth = currentMonth + 1
      this.checkDate = `${currentYear}年${currentMonth}月${currentDay}日`
      this.$emit('handleToday');
    },
    // 点击某一天
    handleClickDay(item) {
      let date = item.date

      let { year: currentYear, month: currentMonth, day: currentDay } = calendar.getNewDate(calendar.getDate(this.time.year, this.time.month, 1));

      let { year, month, day } = calendar.getNewDate(date);

      month = month + 1

      currentMonth = currentMonth + 1

      if (month > currentMonth) {
        this.handleNextMonth()
        this.calendarList = this.visibleCalendar;
      } else if (month < currentMonth) {
        this.handlePrevMonth()
        this.calendarList = this.visibleCalendar;
      }

      this.$forceUpdate();
      this.$emit('handleClickDay', item);
      this.checkDate = `${item.year}年${item.month}月${item.day}日`
    }
  },
  created() {
    let { year: currentYear, month: currentMonth, day: currentDay } = calendar.getNewDate(new Date());
    currentMonth = currentMonth + 1
    this.checkDate = `${currentYear}年${currentMonth}月${currentDay}日`
    this.calendarList = this.visibleCalendar;
    this.calendarType = this.options.calendarType;
  }
}
</script>

<style lang="less">
ul {
  padding: 0;
  margin: 0;
  list-style: none;
}
.cc-calendar {
  width: 100%;
  height: 100%;
  background: #fff;
  box-sizing: border-box;
  .calendar-week {
    display: flex;
    width: 100%;
    height: 46px;
    border: none;
    .week-item {
      width: 14%;
      text-align: center;
      font-size: 14px;
      color: #cccccc;

      font-weight: 600;
    }
  }
  .calendar-view {
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    border-left: 1px solid #f5f5f5;
    border-bottom: 1px solid #f5f5f5;
    .date-view {
      width: 14%;
      height: 90px;
      line-height: 90px;
      border-top: 1px solid #f5f5f5;
      border-right: 1px solid #f5f5f5;
      cursor: pointer;
      .date-day {
        display: block;
        width: 100%;
        font-size: 14px;
        color: #111;
        &:hover {
          background-color: #fafafa;
        }
      }

      .warning-tip {
        position: relative;
        &::after {
          position: absolute;
        }
      }
      .calendar-num {
        margin-top: 6px;
        display: block;
        width: 100%;
        text-align: center;
        font-size: 30px;
        color: #424953;
      }
      .opacity-class {
        font-size: 16px;
        color: #cccccc;
      }
    }
    .todayBg {
      background-color: #ffbc00;
      box-shadow: 0px 2px 12px 0px rgba(255, 188, 0, 0.3);
      > .date-day {
        font-size: 24px;
        color: #272727;
        &:hover {
          background-color: transparent;
        }
      }
    }
  }
}
</style>