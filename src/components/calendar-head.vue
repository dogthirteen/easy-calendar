<template>
  <div class="calendar-header clear">
    <div class="calendar-box">
      <div class="calendar-content" :style="{'text-align': this.headStyle.combination}">
        <span class="calendar-prev" @click="handlePrevMonth"></span>
        <span class="calendar-headDate"> {{this.headOptions.date}} </span>
        <span class="calendar-next" @click="handleNextMonth"></span>
      </div>
      <span class="calendar-today" @click="handleToday()"> 回到今天 </span>
    </div>

  </div>
</template>

<script>
export default {
  props: {
    headOptions: Object
  },
  data() {
    return {
      headStyle: ''
    }
  },
  mounted() {
    this.headStyle = this.headOptions.style;

  },
  methods: {
    // 上一个月
    handlePrevMonth() {
      this.$emit('handlePrevMonth');
    },
    // 下一个月
    handleNextMonth() {
      this.$emit('handleNextMonth');
    },
    // 回到今天
    handleToday() {
      this.$emit('handleToday');
    }
  }
}
</script>

<style lang="less">
.calendar-header {
  margin-bottom: 23px;
  width: 100%;
  .calendar-box {
    display: flex;
    height: 32px;
    line-height: 32px;
    .calendar-content {
      display: flex;
      align-items: center;
      margin-right: 36px;
      .calendar-prev,
      .calendar-next {
        display: inline-block;
        vertical-align: middle;
        width: 8px;
        height: 14px;
        background: url('../assets/img/left.png') no-repeat;
        background-size: contain;
        cursor: pointer;
      }
      .calendar-next {
        background: url('../assets/img/right.png') no-repeat;
        background-size: contain;
      }
    }
    .dispersion {
      width: initial;
      display: inline;
    }
    .calendar-headDate {
      margin: 0 12px;
      font-size: 30px;
      color: #111111;
    }
    .calendar-today {
      width: 90px;
      height: 32px;
      background-color: #006666;
      border-radius: 16px;
      text-align: center;
      font-size: 14px;
      color: #fff;
      cursor: pointer;
    }
    .dispersion-today {
      position: inherit;
    }
    .calendar-center {
      margin: 0 auto;
    }
    .calendar-left {
      float: left;
    }
    .calendar-right {
      float: right;
    }
  }
}
</style>