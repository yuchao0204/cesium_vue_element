<template>
  <div class="viewer">
    <div id = 'cesiumContainer'>{{msg}}</div>
  </div>
</template>

<script>
// import * as Cesium from 'cesium/Cesium';
// import widgets from 'cesium/Widgets/widgets.css';

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  mounted(){
    // this.showtianditu();
    // var viewer = new Cesium.Viewer('cesiumContainer');
  },
  methods:{
    showtianditu(){
      var viewer = new Cesium.Viewer("cesiumContainer", {
        terrainProvider: Cesium.createWorldTerrain(),
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .viewer {
    width: 100%;
    height: 400px;
  }
</style>
