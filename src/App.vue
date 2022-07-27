<template>
  <div ref="app" id="app">
    <div id="help"><p>?</p>
      <div id="helpContainer"> <p> 
        '+' button will create new note.<br>
        'X' button on left corner of note remove given note. <br>
        Click on round button under two notes creates arrow between them.
        </p></div>
    </div>
    <div id="noteAdder" @click="addNote">
    <p>+</p>
    </div>
    <Note @clickArrowPoint="drawArrow($event)" v-if="renderComponent" v-for="note in notes" :ref="'id' + note.id" :id="note.id" :content="note.content" :positionX="note.positionX" :positionY="note.positionY" @remove="removeNote" @changed="changeContent"></note>
 </div>
</template>

<script>
import Note from './Note.vue'
import Arrow from './Arrow.vue'
import Vue from 'vue'

export default {
  name: 'app',
  
  components: {
    Note,
    Arrow
},
  data () {
    return {
      renderComponent: true,
      notes: [],
      id: 0,
      drawing: false,
      startArrow: null,
      endArrow: null
    }
  },    
  methods: {
    forceRerender() {
      this.renderComponent = false;

      this.$nextTick(() => {
        this.renderComponent = true;
      });
    },
    drawArrow(e) {
      if (this.drawing) {
        this.end = e
        this.addArrow(this.start, this.end)
        this.start = null
        this.end = null
      } else {
        this.start = e
      }
      this.drawing = !this.drawing

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
        if(this.notes[i].id === id) {
          newNotes.push({id:this.notes[i].id, content: text, positionX: posX, positionY: posY})
        } else {
          newNotes.push({id:this.notes[i].id, content: this.notes[i].content, positionX: this.notes[i].positionX, positionY: this.notes[i].positionY})
        }
      }
      this.notes = newNotes
    },
    addArrow(start, end) {
      var ComponentClass = Vue.extend(Arrow)
        var arrow = new ComponentClass({
            propsData: { start: start, end: end }
        })
        arrow.$mount()
        this.$refs.app.appendChild(arrow.$el)
    }
  }
}
</script>

<style>
  #app {
    height: 100%;
  }

  #noteAdder {
    text-align: center;
    border-radius: 20px;
    background-color: white;
    margin-top: 90vh;
    margin-left: calc(50% - 75px);
    height: 50px;
    width: 50px;
    position: absolute;
    box-shadow: rgba(0, 0, 0, 0.25) 0px 14px 28px, rgba(0, 0, 0, 0.22) 0px 10px 10px;
  }

  #noteAdder:hover {
    box-shadow: rgba(127, 255, 212, 0.636) 0px 14px 28px, rgba(127, 255, 212, 0.600) 0px 10px 10px;
  }

  p {
    font-weight: bolder;
    font-family: Arial, Helvetica, sans-serif;
  }

  #help {
    text-align: center;
    position: absolute;
    top: 0;
    right: 0;
    border: 1px;
    border-radius: 30px;
    box-shadow: rgba(0, 0, 0, 0.25) 0px 14px 28px, rgba(0, 0, 0, 0.22) 0px 10px 10px;
    width: 50px;
    height: 50px;
  }

  #help:hover {
    box-shadow: rgba(127, 255, 212, 0.636) 0px 14px 28px, rgba(127, 255, 212, 0.600) 0px 10px 10px;
  }

  #help:hover > #helpContainer {
    visibility: visible;
  }

  #helpContainer {
    visibility: collapse;
    width:200px;
    position: absolute;
    top: 8;
    right: 0;
  }
</style>
