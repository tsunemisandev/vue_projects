<template>
  <el-date-picker
    ref="customDatePicker"
    :type="type"
    :size="size"
    :value="value"
    :format="formatter.displayFormat"
    :value-format="formatter.valueFormat"
    v-model="date"
    @change="updateDate"
    @focus="typing = true"
  />
</template>
<script>
import { DatePicker } from 'element-ui'

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
    formatter: { type: Object, reqired: true },
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
        const customParser = this.formatter.parser
        elPicker.parseString = function (value) {
          value = customParser(value)
          console.log(value)
          return originalParseFunction(value)
        }
      }
    })
  },
  created: function () {},
  watch: {
    value(newValue) {
      this.date = newValue
    },
  },
}
</script>
