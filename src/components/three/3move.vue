<script setup>
import { onMounted, ref } from "vue";
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

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
    cube.position.set(5,0,0);
    // cube.position.x = 2;
    scene.add( cube );
    
    const renderer = new THREE.WebGLRenderer();
    renderer.setSize( 800, 768 );
    container.appendChild(renderer.domElement)

    // renderer.render( scene, camera );
    // 创建轨道控制器
    const controls = new OrbitControls(camera, renderer.domElement)
    // 添加坐标轴辅助器
    const axesHelper = new THREE.AxesHelper(5);
    scene.add(axesHelper);
    // 渲染函数
    const renderFn = () => {
        cube.position.x += 0.01;
        if (cube.position.x > 5) {
            cube.position.x = 0;
        }
        renderer.render(scene, camera);
        // 渲染下一帧的时候调用render函数
        requestAnimationFrame(renderFn);
    };
    renderFn();
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
    width: 1080px;
    height: 768px;
    border: 1px solid skyblue;
}
canvas {
    margin: 0 auto;
}
</style>