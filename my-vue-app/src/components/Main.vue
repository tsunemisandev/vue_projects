<script>
import { ref, defineComponent } from 'vue'
import InputDate from './InputDate.vue'
import moment from 'moment'

export default defineComponent({
  components: { InputDate },
  name: 'Main',
  props: {},
  data: function () {
    return {
      date: '2020-02-01',
      ddMMyyyyformatter: {
        displayFormat: 'dd/MM/yyyy',
        valueFormat: 'dd-MM-yyyy',
        parser: function (value) {
          // expects String, Date or an Array of those two
          if (Object.prototype.toString.call(value) === '[object String]') {
            var currentMonth = moment().format('MM')
            var currentYear = moment().format('YYYY')
            value = value.replace(/^[^\d]*(\d{2})[^\d]*$/, `$1/${currentMonth}/${currentYear}`)
            value = value.replace(/^[^\d]*(\d{2})(\d{2})[^\d]*$/, `$1/$2/${currentYear}`)
            value = value.replace(/^[^\d]*(\d{2})\/?(\d{2})\/?(\d{4})[^\d]*$/, '$1/$2/$3')
          }
          if (Array.isArray(value)) {
            value = value.map((date) => expandNumbersToDate(date))
          }
          return value
        },
      },
      yyyyMMddformatter: {
        displayFormat: 'yyyy/MM/dd',
        valueFormat: 'yyyy-MM-dd',
        parser: function (value) {
          console.log('passed ' + value)
          // expects String, Date or an Array of those two
          if (Object.prototype.toString.call(value) === '[object String]') {
            var currentMonth = moment().format('MM')
            var currentYear = moment().format('YYYY')
            value = value.replace(/^[^\d]*(\d{4})\/?(\d{2})\/?(\d{2})[^\d]*$/, '$1/$2/$3')
            value = value.replace(/^[^\d]*(\d{2})(\d{2})[^\d]*$/, `${currentYear}/$2/$1`)
            value = value.replace(/^[^\d]*(\d{2})[^\d]*$/, `${currentYear}/${currentMonth}/$1`)
            console.log(value)
          }
          if (Array.isArray(value)) {
            value = value.map((date) => expandNumbersToDate(date))
          }
          return value
        },
      },
    }
  },
  methods: {
    changeDate: function () {
      this.date = '2020-05-05'
    },
    printDate() {
      console.log(this.date)
    },
  },
  created: function () {},
  setup() {
    const count = ref(0)
    return { count }
  },
})
</script>

<template>
  <div>
    <input-date :value="date" :date.sync="date" :formatter="yyyyMMddformatter"></input-date>
    <el-button @click="changeDate">Update date</el-button>
    <el-button @click="printDate">Print date</el-button>
  </div>
</template>
