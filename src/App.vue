<template>
  <div id="app">
    <AppNavbar @changeMap="updateMap" @openFileDialog="openFileDialog" @showObj="showObj"/>
    <!-- 文件选择框 -->
    <input type="file" accept=".obj,.mtl" multiple ref="fileInput" style="display: none;" @change="handleFileUpload">
    <!-- Map Container -->
    <div id="map" style="height: 100vh;"></div>
  </div>
</template>

<script>
/* global L */
import AppNavbar from './components/AppNavbar.vue';
import {addCubeToMap } from './threeObjLoader';

export default {
  name: 'App',
  components: {
    AppNavbar,
  },

  data(){
    return{
      map: null,
      threeScene: null
    };
  },

  mounted() {
  this.initMap(); // 确保此方法中没有直接使用 import L
  },
  methods: {
    //底图相关操作
    initMap() {
      console.log('Initializing map...'); // 添加调试信息
      //先清除元素内的内容
      document.getElementById('map').innerHTML = '';
      this.map = L.map('map').setView([32.08489, 118.63445], 13);
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        maxZoom: 30,
        attribution: '© OpenStreetMap'
      }).addTo(this.map);
      console.log('Map initialized:', this.map); // 确认地图是否初始化成功
    },
    updateMap(url) {
      if (!this.map) {
        this.initMap();
        // console.error('Map is not initialized yet.');
        return;
      }
      this.map.eachLayer((layer) => {
        if (layer instanceof L.TileLayer) {
          this.map.removeLayer(layer);
        }
      });
      L.tileLayer(url, {
        maxZoom: 30,
        attribution: '© OpenStreetMap'
      }).addTo(this.map);
    },
    clearMap() {
      if (!this.map) {
        console.error('Map is not initialized yet.');
        return;
      }
      // 移除所有图层
      this.map.eachLayer((layer) => {
        this.map.removeLayer(layer);
      });
      // 清除地图实例
      this.map.remove(); // 移除地图
      // 将地图引用设置为 null，方便后续重新初始化
      this.map = null;
      console.log('Map and all layers have been removed.');
    },

    // 文件上传相关操作
    openFileDialog() {
      this.$refs.fileInput.click();
    },
    handleFileUpload(event) {
      const files = event.target.files;
      let objFile = null;
      let mtlFile = null;
      for (const file of files) {
        if (file.name.endsWith('.obj')) {
          objFile = file; // 必须文件
        } else if (file.name.endsWith('.mtl')) {
          mtlFile = file; // 可选文件
        }
      }
      // 确保OBJ文件存在
      if (objFile) {
        this.loadObjFile(objFile, mtlFile);
      } else {
        alert('请上传一个OBJ文件！'); // 提示用户必须上传OBJ文件
      }
    },
    loadObjFile(objFile, mtlFile) {
      // 创建OBJ文件的URL
      const objFilePath = URL.createObjectURL(objFile);
      let mtlFilePath = null;

      // 如果有MTL文件，创建MTL文件的URL
      if (mtlFile) {
        mtlFilePath = URL.createObjectURL(mtlFile);
      }
      console.log('Loading OBJ file:', objFilePath); // 添加调试信息
      console.log('Loading MTL file:', mtlFilePath); // 添加调试信息
    },

    // // 3D模型相关操作
    // initThreeScene(modelPath, mtlPath) {
    //   const container=document.getElementById('map');
    //   this.threeScene=new ThreeScene(container,modelPath,mtlPath); // 创建ThreeScene实例
    // },
    // showObj() {
    //   console.log('Show OBJ file...');
    //   this.clearMap();
    //   const modelPath=require('@/assets/models/iphone/iphone.obj').default;
    //   const mtlPath=require('@/assets/models/iphone/iphone.mtl').default;
    //   this.initThreeScene(modelPath, mtlPath);
    // },
    showObj() {
      console.log('Show OBJ file...');
      this.clearMap();
      addCubeToMap(this.map);
    }
  }
}
</script>

<style>
</style>