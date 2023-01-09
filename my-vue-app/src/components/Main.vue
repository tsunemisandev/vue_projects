<script>
import { ref, defineComponent } from 'vue'
import InputDate from './InputDate.vue'

export default defineComponent({
  components: { InputDate },
  name: 'Main',
  props: {},
  data: function () {
    return {
      date: '01-02-2020',
    }
  },
  methods: {
    changeDate: function () {
      this.date = '05-05-2020'
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
    <input-date :value="date" :date.sync="date"></input-date>
    <el-button @click="changeDate">Update date</el-button>
    <el-button @click="printDate">Print date</el-button>
  </div>
</template>
