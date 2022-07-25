<template>
  <div id="app">
    <div id="noteAdder" @click="addNote">
      <!-- <ColorPicker></ColorPicker> -->
    </div>
    <svg width="1920" height="1080">

      <defs>
        <marker id="arrow" markerWidth="13" markerHeight="13" refx="2" refy="6" orient="auto">
          <path d="M5,5 L0,9 L8,4 L0,0" style="fill:black;" />
        </marker>
      </defs>

      <path id="arrowPath" d="M0,0 L100,150"
          style="stroke:black; stroke-width: 1.25px; fill: none;
            marker-end: url(#arrow);"
      />
    </svg>
    <note v-if="renderComponent" v-for="note in notes" :id="note.id" :content="note.content" :positionX="note.positionX" :positionY="note.positionY" @remove="removeNote" @changed="changeContent"></note>
 </div>
</template>

<script>
import note from './Note.vue'
// import ColorPicker from './ColorPicker.vue'

export default {
  name: 'app',
  
  components: {
    note,
    // ColorPicker
},
  data () {
    return {
      renderComponent: true,
      noteCnt: 0,
      notes: [],
      id: 0
    }
  },    
  mounted() {
    this.setArrowBetweenNotes(100,100,300,0)

  },
  methods: {
    forceRerender() {
      this.renderComponent = false;

      this.$nextTick(() => {
        this.renderComponent = true;
      });
    },
    setArrowBetweenNotes(posXA, posYA, posXB, posYB) {
      let coords = 'M'+posXA+','+posYA+'L'+posXB+','+posYB
      document.getElementById('arrowPath').setAttribute('d', coords)
    },
    addNote() {
      this.notes.push({id: this.id++, content: '', positionX: 0, positionY: 0})
    },
    removeNote(id) {
      this.notes = this.notes.filter((note) => note.id !== id)
      this.forceRerender()
    },
    changeContent(id, text, posX, posY) {
      let newNotes = []

      for(let i = 0; i < this.notes.length; i++) {
        if(this.notes[i].id === id && posX != 0 && posY != 0) {
          newNotes.push({id:this.notes[i].id, content: text, positionX: posX, positionY: posY})
        } else {
          newNotes.push({id:this.notes[i].id, content: this.notes[i].content, positionX: this.notes[i].positionX, positionY: this.notes[i].positionY})
        }
      }
      this.notes = newNotes
    }
  }
}
</script>

<style>
  #app {
    height: 100%;
  }
  #noteAdder {
    border-radius: 20px;
    background-color: white;
    margin-top: 90vh;
    margin-left: calc(50% - 75px);
    height: 40px;
    width: 150px;
    position: absolute;
    box-shadow: rgba(0, 0, 0, 0.25) 0px 14px 28px, rgba(0, 0, 0, 0.22) 0px 10px 10px;
  }
</style>
