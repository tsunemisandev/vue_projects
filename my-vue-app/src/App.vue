<script>
import { defineComponent } from 'vue'
import NumericInput from './components/NumericInput.vue'

export default defineComponent({
  name: 'App',
  components: {
    NumericInput,
  },
  data: {
    numero: null,
  },
  methods: {
    updateValue(val, prop) {
      this[prop] = val
      console.log(prop)
      console.log('Parent received:' + this[prop])
    },
    hello(val) {
      console.log(this.numero)
      console.log('Parent @change' + val)
    },
  },
})
</script>

<template>
  <div id="app">
    <NumericInput
      :disabled="false"
      :decimal="2"
      refName="test"
      @change="hello($event)"
      :initial="numero"
      @input="
        (value) => {
          this.numero = value
        }
      "
    >
      <template slot="append">OLA</template>
    </NumericInput>
  </div>
</template>
