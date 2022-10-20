<script setup>
import { onMounted, ref } from "vue";
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
// 导入动画库
import gsap from "gsap";
import * as dat from "dat.gui";

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
    const geometry = new THREE.BufferGeometry();
    // 一位数组，每3个值设为一个顶点
    const vertices = new Float32Array([
        -1.0, -1.0, 1.0,
        1.0, -1.0, 1.0,
        1.0, 1.0, 1.0,
        1.0, 1.0, 1.0,
        -1.0, 1.0, 1.0,
        -1.0, -1.0, 1.0,
    ])

    geometry.setAttribute('position', new THREE.BufferAttribute(vertices, 3));

    // 创建基础网格材质时可设置颜色 new THREE.MeshBasicMaterial({color: 0x0080ff});
    const material = new THREE.MeshBasicMaterial({color: 0xffff00});
    // 根据几何体合材质创造物体
    const mesh = new THREE.Mesh(geometry, material);
    console.log("MESH", mesh);
    scene.add(mesh);

    console.log('geometry',geometry);

    const renderer = new THREE.WebGLRenderer();
    renderer.setSize( 800, 768 );
    container.appendChild(renderer.domElement)

    // renderer.render( scene, camera );
    // 创建轨道控制器
    const controls = new OrbitControls(camera, renderer.domElement);
    // 设置控制器阻尼，更真实效果，必须在render函数里设置update
    controls.enableDamping = true;

    // 添加坐标轴辅助器
    const axesHelper = new THREE.AxesHelper(5);
    scene.add(axesHelper);
    // 设置时钟
    // const clock = new THREE.Clock();

    // 渲染函数
    const renderFn = (time) => {
        controls.update();
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