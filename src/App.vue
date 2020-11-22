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
          fontWeight: mWeight(el.rangeWeight),
          fontFamily: el.mFont,
          fontSize: el.mSize + 'px',
          color: el.mColor,
          transform: 'translateX(-50%) rotate(' + mRotate(el.rangeRotate) + 'deg)',
        }"
        >
          <span>{{ el.mText }}</span>
      </movable>
    </div>

    <!-- -------------------------------------------------------------- -->
    <!-- -------------------------------------------------------------- -->

    <div class="controlls">
      
      <div class="element-options" :key="el.id" v-for="el in elements">
        <div class="mText">
          <input v-model="el.mText">
        </div>
        <div>
          <label for="select-mFont"></label>
          <select v-model="el.mFont" id="select-mFont">
            <option>Roboto</option>
            <option>Monospace</option>
            <option>Lobster</option>
            <option>Cursive</option>
          </select>
        </div>
        <div class="mColor">
          <label for="select-mColor">Text color: {{ el.mColor }}</label>
          <input id="select-mColor" type="color" v-model="el.mColor">
          <!-- this won't work since color options will be limited -->
        </div>
        <div class="mSize">
          <label for="select-mSize">Font size: {{ el.mSize }}</label>
          <button @click="el.mSize += 2">Increment</button>
          <button @click="el.mSize -= 2">Decrement</button>
        </div>
        <div class="mWeight">
          <label for="select-mWeight">Font weight: {{ mWeight(el.rangeWeight) }}</label>
          <input id="select-mWeight" type="range" v-model="el.rangeWeight">
        </div>
        <div class="mRotate">
          <label for="select-mWeight">Rotation: {{ mRotate(el.rangeRotate) }}</label>
          <br>
          <input id="select-mWeight" type="range" v-model="el.rangeRotate">
        </div>
        <div class="mPosition">
          <button @click="el.mLeft = 350">Center</button>
          <p>X = {{ el.mLeft }}</p>
          <p>Y = {{ el.mTop }}</p>
        </div>
        <button @click="removeElement(el)">Remove</button>
      </div>

      <div class="commands">
        <button @click="addElement">Add New Element</button>
        <!-- <button @click="generateOutput">Generate Output</button> -->
        <button @click="processInput">Take input</button>
        <input v-model="input">
        <br>
        <textarea v-model="output"></textarea>
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
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld
  },

  data() {
    return {
      idOf: 0,
      input: '',
      elements: [
        {
          id: 0,
          mText: "Hello World",
          mFont: "Roboto",
          mColor: "#000000",
          mSize: 32,
          rangeWeight: 40,
          rangeRotate: 0,
          mLeft: 350,
          mTop: 150,
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
    // con() {
    //   console.log("detected")
    // },
    mWeight(i) {
      return i * 8 + 100
    },
    mRotate(i) {
      var r = i * 3.6
      return Math.round(r)
    },
    addElement() {
      const newElement = {
        id: this.elements[this.elements.length - 1].id + 1,
        mText: "Hello World",
        mFont: "Roboto",
        mColor: "#000000",
        mSize: 32,
        rangeWeight: 40,
        rangeRotate: 0,
        mLeft: 350,
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
}

.view {
  position: relative;
  background: #eee;
  width: 700px;
  max-width: 100vw;
  height: 700px;
  left: 50%;
  transform: translateX(-50%);
}

.message {
  /* transform: translateX(-50%) !important; */
}

.controlls {
  position: relative;
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-gap: 1rem;
  width: 668px;
  left: 50%;
  transform: translateX(-50%);
  padding: 1rem;
  background: #333;
}

.element-options {
  background: #222;
  padding: 1rem 1.5rem;
  border-radius: 8px;
}

.mText input {
  width: 100%;
  padding: .25rem .5rem;
  transform: translateX(-0.5rem);
  border-radius: 6px;
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

.commands {

}



</style>










