<template>
  <el-date-picker
    ref="customDatePicker"
    :type="type"
    :size="size"
    :value="value"
    format="dd/MM/yyyy"
    value-format="dd-MM-yyyy"
    v-model="date"
    @change="updateDate"
    @focus="typing = true"
  />
</template>
<script>
import { DatePicker } from 'element-ui'
import moment from 'moment'

export default {
  components: {
    'el-date-picker': DatePicker,
  },
  data() {
    return {
      date: '',
      typing: false,
    }
  },
  props: {
    type: { type: String, default: 'date' },
    size: { type: String, default: 'small' },
    value: { type: String, default: '' },
  },
  computed: {},
  methods: {
    updateDate: function () {
      this.$emit('update:date', this.date)
    },
  },
  mounted: function () {
    this.$nextTick(() => {
      var elPicker = this.$refs.customDatePicker
      if (this.type === 'date' || this.type === 'daterange') {
        // inject custom date input behavior

        const originalParseFunction = elPicker.parseString
        elPicker.parseString = function (value) {
          value = expandNumbersToDate(value)
          console.log(value)
          console.log(originalParseFunction(value))
          return originalParseFunction(value)
        }
      }
    })
  },
  watch: {
    value(newValue) {
      console.log('New ' + newValue)
      this.date = newValue
    },
  },
}

function expandNumbersToDate(value) {
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
}
</script>
