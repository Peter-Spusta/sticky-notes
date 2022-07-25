<template>
  <div ref="note" id="note" :draggable="true" @dragend="changePosition($event)">
    <button @click="$emit('remove',id)">X</button>
    <textarea v-model="text" placeholder="Type here" @change="$emit('changed', id, text, posX, posY)"></textarea> 
    <div id="arrowPoint"></div>
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
        posY: this.positionY
    }
  },
  mounted() {
    this.$refs.note.style.left = this.posX + 'px',
    this.$refs.note.style.top = this.posY + 'px'
  },
  methods: {
    changePosition(event) {
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
  border-radius: 15px;
  width: 15px;
  height: 15px;
  position: absolute;
  left: calc(50%);
  /* top: calc( 100% + 3px); */
}

#arrowPoint:hover > #arrowPoint {
  visibility: visible;
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
    background-color: red;
}
</style>
