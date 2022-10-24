<script setup>
import { onMounted, ref } from "vue";
import * as THREE from "three";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
// 导入动画库
import codingImg from '@/assets/images/JavaScript.png';
import door from '@/assets/images/door.jpeg';
import doorBg from '@/assets/images/doorbg.jpg';

const initScenes = () => {
    let container = document.getElementById('container')
    // 1. 创建场景
    const scene = new THREE.Scene();
    scene.background = codingImg;

    // 2. 创建相机 - 透视相机 PerspectiveCamera
    const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );

    // 3. 设置相机位置
    camera.position.set( 0, 0, 10 );
    // camera.lookAt( 0, 0, 0 );
    scene.add(camera);
    
    // -- 添加物体
    // 导入纹理
    const textureLoader = new THREE.TextureLoader();
    const codingColorTexture = textureLoader.load(codingImg);
    // 纹理加载器
    const doorAplhaTexture = textureLoader.load(doorBg);

    const doorTexture = textureLoader.load(door);
    // 创建几何体
    const cubeGeometry = new THREE.BoxGeometry(1,1,1);
    // 设置偏移
    // codingColorTexture.offset.set(0.5, 0.5); x y
    // 纹理旋转
    // codingColorTexture.rotation = Math.PI / 4;
    // 设置旋转的原点
    // codingColorTexture.center.set(0.5, 0.5);
    // 设置纹理重复 repeat
    codingColorTexture.repeat.set(2,3);
    // 设置纹理重复的模式
    codingColorTexture.wrapT = THREE.MirroredRepeatWrapping;
    codingColorTexture.wrapS = THREE.RepeatWrapping;
    
    // 材质
    // const basicMaterial = new THREE.MeshBasicMaterial({color: '#ffffff', map: codingColorTexture});
    const basicMaterial = new THREE.MeshBasicMaterial({
        color: '#ffffff', 
        map: doorTexture, 
        alphaMap: doorAplhaTexture,
        transparent: true,
        // opacity: 0.5 透明度
        side: THREE.DoubleSide,
    });
    // 物体
    const cube = new THREE.Mesh(cubeGeometry, basicMaterial);
    scene.add(cube);
    // 添加平面
    const plane = new THREE.Mesh(
        new THREE.PlaneGeometry(1, 1),
        basicMaterial
    );
    plane.position.set(3,0,0);
    scene.add(plane);

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