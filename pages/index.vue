<template>
  <div class="container">
    <dynamic
      :comps="comps"
      :emptyView="emptyView"
      @click.native="onClick"
      @customEvent="onCustomEvent"
    />
    <div>
      <button @click="changeView">changeView</button>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { Dynamic } from 'vue-dynamic'

const check = {
  template: '<div>There is nothing now!</div>'
}

export default Vue.extend({
  data: () => ({
    name: 'Mellow',
    comps: [
      {
        template: `<div>Dynamic Rendering</div>`
      }
    ],
    emptyView: check
  }),
  computed: {},
  methods: {
    changeView() {
      this.comps = [
        {
          template: `<div @click="addCount">I'm dynamic load! Click on me to count. {{ count | repeat }}</div>`,
          data: {
            count: 0
          },
          filters: {
            repeat: ['count', 'return count.toString().repeat(2)']
          },
          methods: {
            addCount: [
              'e',
              `console.log(e.type);this.count++;this.$emit('customEvent')`
            ]
          }
        }
      ]
    },
    onClick() {
      console.log('clicked!')
    },
    onCustomEvent() {
      console.log('CustomEvent is supported!')
    }
  },
  components: { Dynamic }
})
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
