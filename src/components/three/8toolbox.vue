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
    const geometry = new THREE.BoxGeometry(1,1,1);
    // 创建基础网格材质时可设置颜色 new THREE.MeshBasicMaterial({color: 0x0080ff});
    const material = new THREE.MeshNormalMaterial();
    const cube = new THREE.Mesh( geometry, material );
    cube.position.set(0,0,0);
    // 缩放
    // cube.scale.set(2,1,1);
    // 旋转
    cube.rotation.set(Math.PI / 4, 0, 0)
    // cube.position.x = 2;
    scene.add( cube );

    // GUI
    const gui = new dat.GUI();
    gui.add(cube.position, 'x').min(0).max(5).step(0.01).name('x轴').onChange((value) => {
        console.log("值",value);
    });
    gui.add(cube.rotation,'y').min(0).max(5);
    // 修改物体颜色
    const colorParams = {
        color: '#ffff00',
        fn: () => {
            gsap.to(cube.position,{x: 5, duration: 2, yoyo: true, repeat: -1})
        }
    };
    gui.addColor(colorParams, 'color').onChange((value) => {
        console.log("颜色修改", value);
        // cube.material.color.set(value);
    })
    gui.add(cube, 'visible').name('显示')
    // 点击按钮出发方法
    gui.add(colorParams, 'fn').name('运动');
    let folder = gui.addFolder('设置立方体');
    folder.add(cube.material, 'wireframe')

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
 
    // resize
    window.addEventListener('resize', () => {
        console.log("监听画面变化");
        // 更新摄像头
        camera.aspect = window.innerWidth / window.innerHeight;
        // 更新摄像机的投影矩阵
        camera.updateProjectionMatrix();
        // 更新渲染器
        renderer.setSize(window.innerWidth, window.innerHeight);
        // 设置渲染器的像素比
        renderer.setPixelRatio(window.devicePixelRatio);
    })
    // 渲染函数
    const renderFn = () => {
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