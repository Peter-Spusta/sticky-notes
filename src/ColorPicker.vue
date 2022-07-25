<template>
  <div>
    <div id="colorPickerButton" @click="() => {visible = !visible}"></div>
    <div v-if="visible" id="colorPickerPallete">
        <div id="color" v-for="color in colors"></div>
    </div>
  </div>
</template>

<script>

export default {

  data () {
    return {
        visible: false,
        colors: ['#008000', '#800000', '#000080']
    }
    
  },    
  methods: {
    getStyle(color) {
        color: color
    }
  },
  
}
</script>

<style>
  #colorPickerButton {
    background-color: #008000;
    height: 20px;
    margin-top: 10px;
    margin-left: 100px;
    width: 30px;
    position: absolute;
  }
  #colorPickerPallete {
    position:absolute;
    bottom: 50px;
    height: 200px;
    width: 200px;
    box-shadow: rgba(0, 0, 0, 0.25) 0px 14px 28px, rgba(0, 0, 0, 0.22) 0px 10px 10px;
  }
  #color {
    height: 20px;
    width: 20px;
    /* background-color: #008000; */
  }
</style>
