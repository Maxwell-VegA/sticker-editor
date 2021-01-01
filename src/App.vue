<template>
  <div id="app">
    <div class="view">
      <movable
        :key="el.id" 
        v-for="el in elements"
        class="message"
        shiftKey="true"
        :posTop="el.mTop"
        :posLeft="el.mLeft"
        bounds="{x:[100,400],y:[100,400]}"
        @start="idOf = el.id"
        @complete="trackLocation"
        :style="{ 
          fontWeight: el.rangeWeight,
          fontFamily: el.mFont,
          fontSize: el.mSize + 'px',
          color: findColor(el.mColor),
          transform: 'translateX(-50%) rotate(' + el.rangeRotate + 'deg)',
        }"
        >
          <span>{{ el.mText }}</span>
      </movable>
    </div>

    <div class="controlls">
      
      <div class="element-options" :key="el.id" v-for="el in elements">
        <div class="mText text-black">
          <input v-model="el.mText" class="rounded-md">
        </div>
        <div class="pt-3">
          <label for="select-mFont">Font:</label>
          <select v-model="el.mFont" id="select-mFont" class="text-black float-right mr-2 rounded-md">
            <option>Roboto</option>
            <option>Monospace</option>
            <option>Lobster</option>
            <option>Cursive</option>
          </select>
        </div>
        <div class="mColor mt-1">
          <label for="select-mColor">Color: </label>
          <select id="select-mColor" v-model="el.mColor" class="text-black float-right mr-2 rounded-md">
            <option :key="index" v-for="(color, index) in colors">{{ color.text }}</option>
          </select>
          <div :key="index" v-for="(type, index) in aveilableColorTypes(el.mColor)" class="ml-4">
            <input type="checkbox" :true-value="type" v-model="el.mColorType" class="">
            <span class="ml-2">{{ type }}</span>
            <br>
          </div>
        </div>
        <div class="mSize mt-1">
          <label for="select-mSize">Size:</label>
          <input type="range" min="100" max="400" v-model="el.mSize" class="float-right">
        </div>
        <div class="mWeight mt-1">
          <label for="select-mWeight">Font weight:</label>
          <input id="select-mWeight" type="range" min="100" max="900" v-model="el.rangeWeight" class="float-right">
        </div>
        <div class="mRotate mt-1">
          <label for="select-mRotate">Rotation: {{ el.rangeRotate }}</label>
          <input id="select-mRotate" type="range" min="0" max="360" v-model="el.rangeRotate" class="float-right">
        </div>
        <div class="mPosition mt-3">
          <button @click="el.mLeft = 400" class="bg-gray-500 p-1 px-3 rounded-md hover:bg-gray-600">Center element</button>
          <button @click="removeElement(el)" class="bg-red-700 p-1 px-3 rounded-md float-right hover:bg-red-800">Remove</button>
          <!-- <p>X = {{ el.mLeft }}</p> -->
          <!-- <p>Y = {{ el.mTop }}</p> -->
        </div>
      </div>

      <div class="commands">
        <button @click="addElement">Add New Element</button>
        <br>
        <input v-model="input" class="text-black">
        <button @click="processInput">Process input</button>
        <br>
        <textarea v-model="output" class="text-black h-40 resize-none mt-3"></textarea>
      </div>
      
      <!-- <div>
        <p>Canvas size: {{ canvas }}</p>
        <button @click="canvas='lg'">Large</button>
        <button @click="canvas='md'">Medium</button>
        <button @click="canvas='sm'">Small</button>
      </div> -->

    </div>
  </div>
</template>

<script>
// import { filter } from 'vue/types/umd'
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  data() {
    return {
      idOf: 0,
      input: '',
      selectedColor: '',
      elements: [
        {
          id: 0,
          mText: "Hello World",
          mFont: "Roboto",
          mColor: "",
          mColorType: "",
          mSize: 144,
          rangeWeight: 400,
          rangeRotate: 0,
          mLeft: 400,
          mTop: 150,
        },
      ],
      colors: [
        {
          text: "Pelēks",
          value: "#808080",
          options: ['samts'],
        },
        {
          text: "Dzeltens",
          value: "#ffff00",
          options: ['gluds', 'samts'],
        },
        {
          text: "Zils",
          value: "#00f",
          options: ['gluds', 'samts'],
        },
        {
          text: "Sudrabs",
          value: "#ffffff",
          options: ['gluds', 'gliters', 'spogulis', 'čūskāda', 'pērļu'],
        },
        {
          text: "Sarkans",
          value: "#ff0000",
          options: ['gluds', 'samts', 'pērļu'],
        },
        {
          text: "Karbons",
          value: "#444",
          options: [],
        },
      ]
    }
  },

  computed: {
    price() {
      return 0
    },
    output() {
      return JSON.stringify(this.elements)
    }
  },
  
  methods: { 
    findColor(name) {
      const obj = this.colors.find(color => color.text === name)
      return obj ? obj.value : "#000"
    },
    aveilableColorTypes(name) {
      const obj = this.colors.find(color => color.text === name)
      return obj ? obj.options : null
    },
    addElement() {
      const newElement = {
        id: this.elements[this.elements.length - 1].id + 1,
        mText: "Hello World",
        mFont: "Roboto",
        mColor: "melns",
        mColorType: "",
        mSize: 144,
        rangeWeight: 400,
        rangeRotate: 0,
        mLeft: 400,
        mTop: 150,
      }
      this.elements = [...this.elements, newElement];
    },
    removeElement(el) {
      if (this.elements.length > 1) {
        this.elements = this.elements.filter(element => element != el);
      }
    },
    trackLocation(event) {
      this.elements[this.idOf].mLeft = event.css.left
      this.elements[this.idOf].mTop = event.css.top
    },
    // generateOutput() {
    //    console.log(JSON.stringify(this.elements))
    // },
    processInput() {
      if (this.input != '') {
        this.elements = JSON.parse(this.input);
        console.log(1)
      }
    }
  }
}

</script>

<style>
* {
  margin: 0px;
  padding: 0px;
}

#app {
  font-family: Roboto, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  overflow-x: hidden;
  background: #222;
  color: white;
  height: 100vh;
  /* display: grid; */
  /* grid-template-columns: 1fr 1fr; */
}

.view {
  position: relative;
  background: #eee;
  width: 800px;
  max-width: 100vw;
  height: 533px;
  left: 50%;
  transform: translateX(-50%);
  overflow: hidden
}

.message {
  /* transform: translateX(-50%) !important; */
  margin-right: -48rem;
  color: #000;
}

.controlls {
  position: relative;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 1rem;
  width: 800px;
  left: 50%;
  transform: translateX(-50%);
  padding: 1rem;
  background: #333;
}

.element-options, .commands {
  background: #222;
  padding: 1rem 1.5rem;
  border-radius: 8px;
}

.mText input {
  width: 100%;
  padding: .25rem .5rem;
  transform: translateX(-0.5rem);
  border: none;
}

.mText select {
  width: 100%;
  padding: .25rem .5rem;
  transform: translateX(-0.5rem);
  border-radius: 6px;
  border: none;
}

textarea {
  height: 8rem;
  width: 100%;
  resize: vertical;
}

</style>










