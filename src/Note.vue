<template>
  <div @click="$emit('endArrowPoint', thisNote)" ref="note" id="note" :draggable="true" @dragend="changePosition($event);$emit('changed', id, text, posX, posY)">
    <button @click="$emit('remove',id)">X</button>
    <textarea @mouseup="handleTAChange" v-model="text" placeholder="Type here" @change="$emit('changed', id, text, posX, posY)"></textarea> 
    <div @click="$emit('clickArrowPoint', thisNote)" id="arrowPoint"></div>
  </div>
</template>

<script>

export default {
  props: {
    id: Number,
    content: String,
    positionX: Number,
    positionY: Number
  },
  data () {
    return {
        text: this.content,
        posX: this.positionX,
        posY: this.positionY,
        height: 0,
        width: 0,
        thisNote: this
    }
  },
  mounted() {
    this.height = this.$refs.note.clientHeight
    this.width = this.$refs.note.clientWidth
    this.$refs.note.style.left = this.posX + 'px'
    this.$refs.note.style.top = this.posY + 'px'
  },
  methods: {
    handleTAChange() {
      this.height = this.$refs.note.clientHeight
      this.width = this.$refs.note.clientWidth
    },
    changePosition(event) {
      console.log(this.height)
        this.$refs.note.style.left = event.pageX + 'px';
        this.$refs.note.style.top = event.pageY + 'px';
        this.posX = parseInt(this.$refs.note.style.left)
        this.posY = parseInt(this.$refs.note.style.top)
    },
  },
}
</script>

<style scoped>
#note {
  background-color: cadetblue;
  position: absolute;
  box-shadow: rgba(0, 0, 0, 0.25) 0px 14px 28px, rgba(0, 0, 0, 0.22) 0px 10px 10px;
}

#note:hover > button {
    visibility: visible;
}

#note:hover > #arrowPoint {
  visibility: visible;
}

#arrowPoint {
  visibility: collapse;
  border: 1px;
  border-style: solid;
  border-radius: 10px;
  width: 24px;
  height: 15px;
  position: absolute;
  left: calc(50% - 12px);
}

#arrowPoint:hover {
  cursor: pointer;
  background-color: rgb(127, 255, 212);
  border: none;
  box-shadow: rgba(127, 255, 212,  0.30) 0px 54px 55px, rgba(127, 255, 212, 0.15) 0px -12px 30px, rgba(127, 255, 212,  0.15) 0px 4px 6px, rgba(127, 255, 212,  0.20) 0px 12px 13px, rgba(127, 255, 212,  0.11) 0px -3px 5px;
}

textarea {
    border: none;
    height: 100%;
    width: 100%;
    min-height: 200px;
    min-width: 100px;
    background-color: transparent;
    box-sizing: border-box;
    font-size: large;
}

button {
    position:absolute;
    right: 0;
    top: 0;
    border: none;
    background-color: transparent;
    visibility: collapse;
    cursor: pointer;
    background-color: red;
}
</style>
