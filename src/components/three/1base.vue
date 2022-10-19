<script setup>
import { onMounted, ref } from "vue";
import * as THREE from "three";

const initScenes = () => {
    let container = document.getElementById('container')
    // 1. 创建场景
    const scene = new THREE.Scene();

    // 2. 创建相机 - 透视相机 PerspectiveCamera
    const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );

    // 3. 设置相机位置
    camera.position.set( 0, 0, 10 );
    // camera.lookAt( 0, 0, 0 );
    scene.add(camera);
    
    // -- 添加物体
    // 创建几何体
    const geometry = new THREE.BoxGeometry(1,1,1);
    const material = new THREE.MeshBasicMaterial( {color: 0x00ff00} )
    const cube = new THREE.Mesh( geometry, material );
    scene.add( cube );
    
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize( 500, 500 );
    container.appendChild(renderer.domElement)

    renderer.render( scene, camera );

}

onMounted(() => {
    initScenes();
    
});
</script>

<template>
  <div id="container" >
  </div>

</template>
<style>
#container {
    width: 600px;
    height: 800px;
    border: 1px solid skyblue;
}
canvas {
    margin: 0 auto;
}
</style>