<template>
  <div class="viewer">
    <div id = 'cesiumContainer'></div>
  </div>
</template>

<script>
import * as Cesium from 'cesium/Cesium';
import widgets from 'cesium/Widgets/widgets.css';

export default {
  name: 'HelloWorld',
  data () {
    return {
    }
  },
  mounted(){
    this.showtianditu();
  },
  methods:{
    showtianditu(){
      var viewer = new Cesium.Viewer("cesiumContainer", {
        terrainProvider: Cesium.createWorldTerrain(),
      });

      var tileset = new Cesium.Cesium3DTileset({
          url: "http://localhost/data/tileset.json",
          classificationType: Cesium.ClassificationType.CESIUM_3D_TILE,
      });
      tileset.readyPromise.then(function () {
        console.log("tileset",tileset);
        var boundingSphere = tileset.boundingSphere;
        viewer.camera.viewBoundingSphere(boundingSphere, new Cesium.HeadingPitchRange(0.0, -0.5, boundingSphere.radius));
        viewer.camera.lookAtTransform(Cesium.Matrix4.IDENTITY);
      }).otherwise(function (error) {
        throw (error);
      });



      viewer.scene.primitives.add(tileset);
      
      //设置初始位置
      viewer.camera.setView({
          destination: Cesium.Cartesian3.fromDegrees(121.133199,31.164338, 1000)
      });
    },
    //单体化模型
    test1(){
      Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJmNzA1N2JhMy1kMWIyLTRlZTctODM3Mi0xMTQzZGJhMDM1ODMiLCJpZCI6MzMwOTcsInNjb3BlcyI6WyJhc3IiLCJnYyJdLCJpYXQiOjE1OTc5ODIwNjJ9.WVv9hS-xIswal5Dn8X28p2F4DHbHGvU-UXnodl7uf4k';
      var viewer = new Cesium.Viewer("cesiumContainer", {
        terrainProvider: Cesium.createWorldTerrain(),
      });
      //取消双击事件 viewer.cesiumWidget.screenSpaceEventHandler.removeInputAction(Cesium.ScreenSpaceEventType.LEFT_DOUBLE_CLICK);
      //设置homebutton的位置
      Cesium.Camera.DEFAULT_VIEW_RECTANGLE = Cesium.Rectangle.fromDegrees(110.15, 34.54, 110.25, 34.56);//Rectangle(west, south, east, north)
      //设置初始位置
      // viewer.camera.setView({
      //     destination: Cesium.Cartesian3.fromDegrees(121.133199,31.164338, 1000)
      // });
      var a=new Cesium.Cesium3DTileset({
        url: Cesium.IonResource.fromAssetId(144932),
        //classificationType: Cesium.ClassificationType.CESIUM_3D_TILE,
      });
      a.style = new Cesium.Cesium3DTileStyle({
        color: "rgba(255, 0, 0, 0.5)",
      });
      viewer.scene.primitives.add(a);
      viewer.zoomTo(a);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .viewer {
    width: 100%;
    height: 390px;
  }
</style>
