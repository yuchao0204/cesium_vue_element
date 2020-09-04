<template>
  <div class="mainWin">
    <div id="cesiumContainer">
    </div>
    <div >
      <div id="loadingOverlay" style="float:left"><h2>Loading...</h2></div>
      <!-- <div id="toolbar"></div> -->
      <div id="topDiv" style="float:left;width:400px"></div>
      <div class="class1" title="智慧党建" style="float:left">
        <p style="margin-top: 5px;margin-bottom: 5px;text-align:center">智慧党建</p>
      </div>
      <div class="class1" title="智慧安防" style="float:left">
        <p style="margin-top: 5px;margin-bottom: 5px;text-align:center">智慧安防</p>
      </div>
      <div class="class1" title="智慧" style="float:left">
        <p style="margin-top: 5px;margin-bottom: 5px;text-align:center">智慧XX</p>
      </div>
    </div>
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
    console.log("Cesium VERSION=",Cesium.VERSION);
    Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJmNzA1N2JhMy1kMWIyLTRlZTctODM3Mi0xMTQzZGJhMDM1ODMiLCJpZCI6MzMwOTcsInNjb3BlcyI6WyJhc3IiLCJnYyJdLCJpYXQiOjE1OTc5ODIwNjJ9.WVv9hS-xIswal5Dn8X28p2F4DHbHGvU-UXnodl7uf4k';
    var viewer = new Cesium.Viewer('cesiumContainer',{
      timeline:false,
      animation:false,
      baseLayerPicker:false,
      fullscreenButton:false,
      geocoder:false,
      // homeButton:false,
      infoBox:false,
      sceneModePicker:false,
      selectionIndicator:false,
      navigationHelpButton:false
    });
    viewer._cesiumWidget._creditContainer.style.display = "none";
    console.log("Cesium VERSION=",Cesium.VERSION);
    var tileset = new Cesium.Cesium3DTileset({
      url: "http://localhost:80/data/tileset.json",
      // classificationType: Cesium.ClassificationType.CESIUM_3D_TILE,
    });
    console.log("Cesium test");
    viewer.scene.primitives.add(tileset);
    //设置初始位置
    viewer.camera.setView({
        destination: Cesium.Cartesian3.fromDegrees(121.133199,31.164338, 1000)
    });
    viewer.homeButton.viewModel.command.beforeExecute.addEventListener(function(e) {
      e.cancel = true;
      viewer.camera.flyTo({
        destination: Cesium.Cartesian3.fromDegrees(121.1298356,31.1655295, 1000)
      });
    });
    let topDiv = document.getElementById("topDiv");
    topDiv.innerHTML="点击坐标的位置为：经度121.12983，纬度31.16552";
    this.addLabels(viewer);
    this.addMouseEvent(viewer);
    document.getElementById("loadingOverlay").style.display="none";
  },
  methods:{
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
        url: Cesium.IonResource.fromAssetId(145408),
        //classificationType: Cesium.ClassificationType.CESIUM_3D_TILE,
      });
      a.style = new Cesium.Cesium3DTileStyle({
        color: "rgba(255, 0, 0, 0.5)",
      });
      viewer.scene.primitives.add(a);
      viewer.zoomTo(a);
    },
    addLabels(viewer){
      viewer.entities.add({
        // name : '风机设备',
        // code:"123456789",
        position : Cesium.Cartesian3.fromDegrees(121.1298356,31.1655295),
        point : { //点
            pixelSize : 5,
            color : Cesium.Color.RED,
            outlineColor : Cesium.Color.WHITE,
            outlineWidth : 2
        },
        label : { //文字标签
            text : '张江·人工智能智慧园区',
            font : '14pt monospace',
            style : Cesium.LabelStyle.FILL_AND_OUTLINE,
            outlineWidth : 2,
            verticalOrigin : Cesium.VerticalOrigin.BOTTOM, //垂直方向以底部来计算标签的位置
            pixelOffset : new Cesium.Cartesian2( 0, -9 )   //偏移量
        }
      });
    },
    addMouseEvent(viewer){
      var handler = new Cesium.ScreenSpaceEventHandler(viewer.scene.canvas);
      handler.setInputAction(function(click){
        var pick = viewer.scene.pick(click.position);
        //选中某模型   pick选中的对象
        if(pick && pick.id){
          viewer.zoomTo( viewer.entities );
        }
        //获取鼠标位置，camera.pickEllipsoid()返回一个cartesian类型位置
        var my_ellipsoid = viewer.scene.globe.ellipsoid;
        let click_position =  viewer.scene.camera.pickEllipsoid(click.position,my_ellipsoid);
        //位置数据转换只地理数据类型
        let carto_position = my_ellipsoid.cartesianToCartographic(click_position);
        //cesium函数转换至地理数据类型的经纬度    
        let longitude_x = Cesium.Math.toDegrees(carto_position.longitude).toFixed(5);
        let longitude_y = Cesium.Math.toDegrees(carto_position.latitude).toFixed(5);
        //topDiv是html中的div
        let topDiv = document.getElementById("topDiv");
        topDiv.innerHTML="点击坐标的位置为：经度"+longitude_x+"，纬度"+longitude_y;
      }, Cesium.ScreenSpaceEventType.LEFT_CLICK);
    },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#cesiumContainer {
  width: 100%;
  height: 90%;
}
.class1{
  border-radius: 5px;
  height: 32px;
  width: 90px;
  margin-left:5%;
  /* position: absolute; */
  /* left: 20%; */
  /* top: 80%; */
  /* background: #303336 url(../images/ploughuav.png); */
  background-color:#3280FC;
  border: 1px solid #303336;
  background-size: cover;
  z-index:6;
  cursor:pointer;
}
.class1:hover{
  color: #fff;
  fill: #fff;
  /* background: #303336 url(../images/ploughuav.png); */
  background-size: cover;
  border-color: #aef;
  box-shadow: 0 0 8px #fff;
}
.mainWin{
  position: absolute;
  top: 0;
  left: 0;
  width: 90%;
  min-height: 100%;
  height: auto;
  background: #000000;
}
</style>
