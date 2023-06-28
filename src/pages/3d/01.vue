<!--
 * @Author: zhangchao
 * @Date: 2023-06-28 21:50:24
 * @LastEditors: zhangchao
 * @LastEditTime: 2023-06-28 22:16:49
 * @Description: file content
-->
<route>
{
  meta: {
    layout: 'headless'
  }
}
</route>
<template>
  <div>
    <div class="contain">
      <canvas id="cc"></canvas>
    </div>
  </div>
</template>

<script setup>
let scene,
  camera,
  controls,
  carmodel,
  mixer, // 动画混合器
  renderer,
  paintBNames,
  envMap,
  envMapboli,
  paintANames
import * as THREE from 'three'
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls'
// GLTF组件
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader'

import { reactive, toRefs, onMounted } from 'vue'

const state = reactive({
  count: 0,
})
function initRender() {
  renderer = new THREE.WebGLRenderer({
    canvas: document.querySelector('#cc'),
    antialias: true,
    alpha: true,
  })

  renderer.setSize(window.innerWidth, window.innerHeight)
  renderer.gammaInput = true
  renderer.gammaOutput = true
  renderer.toneMappingExposure = 0.9
  renderer.toneMapping = THREE.LinearToneMapping
  renderer.toneMappingWhitePoint = 1
  //   document.body.appendChild(renderer.domElement);
  //   初始化场景
  scene = new THREE.Scene()

  //初始化相机
  camera = new THREE.PerspectiveCamera(
    45,
    window.innerWidth / window.innerHeight,
    1,
    8000
  )
  camera.position.set(-1398, 733, 685)
}
function initControls() {
  controls = new OrbitControls(camera, renderer.domElement)
  controls.target.set(0, 100, 0)
  // 如果使用animate方法时，将此函数删除
  //controls.addEventListener( 'change', render );
  // 使动画循环使用时阻尼或自转 意思是否有惯性
  controls.enableDamping = true
  controls.dampingFactor = 0.15
  controls.panSpeed = 0.1
  controls.zoomSpeed = 0.2
  controls.rotateSpeed = 0.09
  //动态阻尼系数 就是鼠标拖拽旋转灵敏度
  //controls.dampingFactor = 0.25;
  //是否可以缩放
  controls.enableZoom = true
  //是否自动旋转
  controls.autoRotate = false
  //设置相机距离原点的最远距离
  controls.minDistance = 1450
  //设置相机距离原点的最远距离
  controls.maxDistance = 2500
  //是否开启右键拖拽
  controls.enablePan = true
  controls.minPolarAngle = 0
  controls.maxPolarAngle = Math.PI * 0.45
  controls.minAzimuthAngle = -Infinity
  controls.maxAzimuthAngle = Infinity
}
function initLight() {
  let ambientLight = new THREE.AmbientLight(0xffffff, 1)
  scene.add(ambientLight)
  let hemiLight = new THREE.HemisphereLight(0xffffff, 0xffffff, 0.65)
  hemiLight.position.set(0, 500, 0)
  scene.add(hemiLight)
  let hemiLightHelper = new THREE.HemisphereLightHelper(hemiLight, 10)
  let light = new THREE.DirectionalLight(0xffffff, 0.75)
  light.position.set(-400, 800, 0)
  light.target
  scene.add(light)
  let spotLight = new THREE.SpotLight(0xffffff, 1, 1600)
  spotLight.position.set(15, 600, 1000)
  spotLight.angle = Math.PI / 7
  spotLight.penumbra = 0.12
  spotLight.decay = 2
  spotLight.lookAt(0, 0, 0)
  scene.add(spotLight)
}
function rend3ds() {
  let geometry = new THREE.BoxGeometry(100, 100, 100) //创建一个立方体几何对象Geometry
  let material = new THREE.MeshLambertMaterial({
    color: 0x0000ff,
  }) //材质对象Material
  let mesh = new THREE.Mesh(geometry, material) //网格模型对象Mesh
  scene.add(mesh) //网格模型添加到场景中
}
function render() {
  if (!renderer) {
    //解决vue bug
    location.reload()
  } else {
    renderer.setClearColor(0xb9d3ff, 1) //设置背景颜色
    renderer.render(scene, camera)
    requestAnimationFrame(render) //请求再次执行渲染函数render
    console.log('render', renderer)
    console.log(scene, camera)
    // 动画混合器
  }
}
onMounted(() => {
  console.log('onMounted')
  initRender()

  rend3ds()
  initLight()
  initControls()
  //   开始
  render()
})
</script>

<style lang="scss" scoped>
#cc {
  width: 500px;
  height: 500px;
  background-color: antiquewhite;
}
</style>

