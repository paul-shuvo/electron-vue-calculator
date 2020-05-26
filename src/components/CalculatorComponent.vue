<template>
  <div class="flex justify-center">
    <div class="flex flex-col bg-black text-orange-600 font-mono w-full h-full rounded-lg" >
      <div class="flex justify-end" style="-webkit-app-region: drag;">
        <button class="no-outline" id="btn-minimize" style="-webkit-app-region: no-drag;" @click="minimize()">
          <img class="w-5 h-4 bg-blue-700  hover:bg-blue-400" src="./../assets/images/icons8-minimize-window-24.png"/>
        </button>
        <button class="rounded-tr-lg no-outline" id="btn-close" style="-webkit-app-region: no-drag;" @click='closeWindow()'>
          <img class="w-2 h-2 bg-red-900 mr-3 mt-1 hover:bg-red-600" src="./../assets/images/icons8-close-window-26.png"/>
        </button>
      </div>
      <div class="bg-black m-2 items-end px-4 pt-2 rounded-lg">
        <div class="h-12 text-right">{{result}}</div>
        <div class="h-8 text-right text-2xl">{{x}}</div>
      </div>

      <div class="flex flex-col ">
        <div>
          <button v-for="item in btn_group" :key="item.name" class="w-1/4 h-16 no-outline hover:bg-gray-900 font-bold" :class="{ 'rounded-bl-lg': item.name == '.', 'rounded-br-lg': item.name == '+' }" @click="handler(item)">
            {{ item.name }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const { BrowserWindow } = require('electron').remote

export default {
  name: 'CalculatorComponent',
  data () {
    return {
      btn_group: [
        { name: 'AC', key: 'Delete' },
        { name: ')', key: ')' },
        { name: '(', key: '(' },
        { name: 'Del', key: 'Backspace' },
        { name: '7', key: '7' },
        { name: '8', key: '8' },
        { name: '9', key: '9' },
        { name: '/', key: '/' },
        { name: '4', key: '4' },
        { name: '5', key: '5' },
        { name: '6', key: '6' },
        { name: '*', key: '*' },
        { name: '1', key: '1' },
        { name: '2', key: '2' },
        { name: '3', key: '3' },
        { name: '-', key: '-' },
        { name: '.', key: '.' },
        { name: '0', key: '0' },
        { name: '=', key: 'Enter' },
        { name: '+', key: '+' }
      ],
      x: '',
      checkEnter: false,
      nums: ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0', '.', '(', ')'],
      operators: ['/', '*', '+', '-'],
      result: 0
    }
  },
  mounted () {
    window.addEventListener('keydown', this.handler)
  },
  beforeDestroy () {
    window.removeEventListener('keydown', this.handler)
  },
  methods: {
    evaluate: function evaluate () {
      let handledResult = 0
      try {
        // eslint-disable-next-line no-eval
        handledResult = eval(this.x)
      } catch (SyntaxError) {
        console.log(SyntaxError.message)
        return
      }
      if (Number.isInteger(handledResult)) {
        this.result = handledResult
      } else {
        this.result = parseFloat(handledResult.toString().split('.')[0] + '.' + handledResult.toString().split('.')[1].slice(0, 5))
      }
    },
    minimize: function () {
      BrowserWindow.getFocusedWindow().minimize()
    },
    closeWindow: function () {
      BrowserWindow.getFocusedWindow().close()
    },
    handler: function (event) {
      const key = event.key
      console.log(key)
      if (this.nums.indexOf(key) > -1 || this.operators.indexOf(key) > -1) {
        this.x += key
      }

      if (key === 'Backspace') {
        this.x = this.x.slice(0, -1)
        this.evaluate()
      } else if (key === 'Enter' || this.nums.indexOf(key) > -1) {
        this.evaluate()
        if (key === 'Enter') {
          this.x = this.result.toString()
          this.result = ''
          this.checkEnter = true
        }
      } else if (key === 'Delete') {
        this.x = ''
        this.result = 0
      }
    }
  },
  props: {
    msg: String
  }
}
</script>

<style scoped>
  .no-outline {
    outline: 0!important;
  }
</style>
