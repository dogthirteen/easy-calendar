<template>
  <datePicker 
    :options="calendarArr" 
    class="calendar" 
    @handleClickDay="handleClickDay" 
    @handlePrevMonth="handlePrevMonth"
    @handleNextMonth="handleNextMonth"
  />
  
</template>

<script>
import datePicker from './components/date-picker'
export default {
  name: 'date-picker',
  data () {
    return {
      calendarArr: {
          type: 'combination',
          headStyle: {
            todayBtn: 'left',
            combination: 'left',
            checkBtn: 'left',
          },
          viewStyle: {
            day: 'center'
          },
          calendarData: []
        },
    }
  },
  components: {
    datePicker
  },
  methods: {
    handleClickDay () {

    },
    handlePrevMonth () {

    },
    handleNextMonth () {
      
    }
  }
}
</script>

<style>

</style>
