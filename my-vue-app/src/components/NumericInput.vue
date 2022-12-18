<script>
import { ref, defineComponent } from 'vue'

export default defineComponent({
  name: 'NumericInput',
  props: {
    initial: Number,
    refName: String,
    decimal: Number,
  },
  data: function () {
    return {
      numero: null,
    }
  },
  methods: {
    onBlurUpdateWithComma(event) {
      const value = event.target.value
      if (isNaN(value)) {
        this.numero = ''
        this.$emit('input', this.numero)
        return
      }
      this.$emit('input', this.numero)
      this.numero = this.format(value.toString())
    },
    format(value) {
      return Number(value.toString()).toLocaleString(undefined, {
        minimumFractionDigits: this.decimal,
        maximumFractionDigits: this.decimal,
      })
    },
    onFocusRemoveComma(event) {
      const value = event.target.value
      if (isNaN(value)) {
        this.numero = ''
        return
      }
      this.numero = value.toString().replaceAll(',', '')
      this.$nextTick(() => {
        this.$refs[this.refName].select()
      })
    },
  },
  created: function () {
    this.numero = this.format(this.initial)
  },
  setup() {
    const count = ref(0)
    return { count }
  },
})
</script>

<template>
  <el-input
    v-bind="$attrs"
    :ref="refName"
    v-on="$listeners"
    v-model="numero"
    @blur="onBlurUpdateWithComma"
    @focus="onFocusRemoveComma"
  >
    <template v-for="(_, name) in $scopedSlots" v-slot:[name]="{ item }">
      <slot :name="name" v-bind:item="item" />
    </template>
  </el-input>
</template>
