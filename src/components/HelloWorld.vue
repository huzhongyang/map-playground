<script setup lang="ts">
import type { Canvas } from 'fabric/fabric-impl'
import { onMounted, ref, shallowRef } from 'vue'
import AMapLoader from '@amap/amap-jsapi-loader'
import '@amap/amap-jsapi-types'
import { fabric } from 'fabric'

const mapContainerRef = ref<HTMLDivElement>()
const GDMAP = shallowRef<{
  AMap: typeof AMap
  map: typeof AMap.Map
}>({
  AMap: {},
  map: {},
})
const fabricCanvasVisible = ref(false)
let canvas: Canvas
const currentEditeId = ref<string>('')
const drawnImg: {
  [key: string]: {
    marker?: typeof AMap.ElasticMarker
    imgHeight: number
    imgWidth: number
    x: number
    y: number
    id: string
    rotate: number
    fitZoom: number
  }
} = {}
onMounted(() => {
  initMap()

  canvas = new fabric.Canvas('fabric-canvas', {
    width: window.innerWidth,
    height: window.innerHeight,
    isDrawingMode: false,
    selection: false,
  })
  canvas.on('mouse:up', (e) => {
    if (e.target)
      return
    console.log('mouse:up')
    console.log(e)

    renderOnMap(drawnImg[currentEditeId.value])
    canvas.clear()
    fabricCanvasVisible.value = false
  })
  canvas.on('object:modified', (e) => {
    console.log('object:modified')
    console.log(e)
    // save
    const target = e.target! as fabric.Image
    const imgWidth = target.width! * target.scaleX!
    const imgHeight = target.height! * target.scaleY!
    const { x, y } = e.target!.getPointByOrigin('center', 'center')
    console.log({
      x: x!,
      y: y!,
      imgWidth,
      imgHeight,
    })
    drawnImg[currentEditeId.value] = {
      ...drawnImg[currentEditeId.value],
      x: x!,
      y: y!,
      imgWidth,
      imgHeight,
      rotate: target.angle!,
    }
    console.log(drawnImg)
  })
})

function handleImgDragEnd(e: DragEvent) {
  const { width: imgWidth, height: imgHeight } = (e.target as HTMLImageElement).getBoundingClientRect()
  const { x, y } = {
    x: e.clientX,
    y: e.clientY,
  }

  const id = crypto.randomUUID()
  renderOnCanvas({
    imgWidth,
    imgHeight,
    x,
    y,
    id,
    rotate: 0,
    fitZoom: GDMAP.value.map.getZoom(),
  })
  drawnImg[id] = {
    imgHeight,
    imgWidth,
    x,
    y,
    id,
    rotate: 0,
    fitZoom: GDMAP.value.map.getZoom(),
  }
  currentEditeId.value = id
}

function renderOnMap(img: Img) {
  const { imgWidth, imgHeight, x, y, id, rotate } = img
  const zoomStyleMapping1 = { // 表示地图级别与styles中样式对应关系的映射
    10: 0,
    11: 0,
    12: 0,
    13: 0,
    14: 0,
    15: 0,
    16: 0,
    17: 0,
    18: 0,
    19: 0,
    20: 0,
  }
  const position = GDMAP.value.map.containerToLngLat([x, y])
  const marker = new GDMAP.value.AMap.ElasticMarker({
    position,
    zooms: [10, 20],
    styles: [
      {
        icon: {
          img: '/static/img/maker-1.png',
          size: [imgWidth, imgHeight],
          ancher: [-imgWidth / 2, -imgHeight / 2],
          fitZoom: img.fitZoom, // 最合适的级别，在此级别下显示为原始大小
          scaleFactor: 2, // 地图放大一级的缩放比例系数
          maxScale: 30, // 最大放大比例
          minScale: 0.001, // 最小放大比例
        },
      },
    ],
    zoomStyleMapping: zoomStyleMapping1,
    extData: {
      id,
    },
  })
  marker.setAngle(rotate)
  drawnImg[id] = {
    marker,
    imgHeight,
    imgWidth,
    x,
    y,
    id,
    rotate,
    fitZoom: img.fitZoom,
  }

  marker.on('click', (e: any) => {
    // 隐藏当前 marker
    marker.hide()
    console.log('click marker')
    console.log(e)
    const id = e.target.getExtData().id
    console.log(drawnImg[id])
    console.log(drawnImg[id].x, drawnImg[id].y)
    let position = e.target.getPosition()
    GDMAP.value.map.setZoomAndCenter(drawnImg[id].fitZoom, position, true)
    position = drawnImg[id].marker.getPosition()
    const { x, y } = GDMAP.value.map.lngLatToContainer(position)
    console.log(`x: ${x}, y: ${y}`)
    drawnImg[id] = {
      ...drawnImg[id],
      x,
      y,
    }
    renderOnCanvas(drawnImg[id])
  })
  GDMAP.value.map.add([marker])
}

interface Img {
  imgWidth: number
  imgHeight: number
  x: number
  y: number
  id: string
  rotate: number
  fitZoom: number
}

function renderOnCanvas(imgInfo: Img) {
  console.log(canvas)
  const { imgWidth, imgHeight, x, y } = imgInfo
  fabric.Image.fromURL('/static/img/maker-1.png', (img) => {
    img.scaleToWidth(imgWidth)
    img.scaleToHeight(imgHeight)
    img.set({
      left: x,
      top: y,
      angle: imgInfo.rotate,
    })
    img.setPositionByOrigin(new fabric.Point(x, y), 'center', 'center')
    img.hasBorders = false
    canvas.add(img).calcOffset()
    canvas.setActiveObject(img)
  })
  fabricCanvasVisible.value = true
}

function handleShowCustomMap() {
}

const currentZoom = ref(10)

function initMap() {
  AMapLoader.load({
    key: 'dc86960982b43599bb32467299efb250',
    version: '2.0',
    plugins: ['AMap.ElasticMarker'],
  }).then((AMap) => {
    GDMAP.value.AMap = AMap
    GDMAP.value.map = new AMap.Map(mapContainerRef.value as HTMLDivElement, {
      zoom: 10,
      zooms: [10, 15],
      center: [116.39, 39.9],
    })
    GDMAP.value.map.on('zoomchange', () => {
      currentZoom.value = GDMAP.value.map.getZoom()
    })
  }).catch()
}

function handleTextDragEnd() {

}

// 创建Object3DLayer图层
const object3Dlayer = new AMap.Object3DLayer()
GDMAP.value.map.add(object3Dlayer)

let druckMeshes, cityMeshes

GDMAP.value.map.plugin(['AMap.GltfLoader'], () => {
  const urlCity = 'https://a.amap.com/jsapi_demos/static/gltf-online/shanghai/scene.gltf'
  const urlDuck = new URL('./test.gltf', import.meta.url).href
  console.log(urlDuck)
  const paramCity = {
    position: new GDMAP.value.AMap.LngLat(121.510909, 31.233366), // 必须
    scale: 3580, // 非必须，默认1
    height: 1800, // 非必须，默认0
    scene: 0, // 非必须，默认0
  }

  const paramDuck = {
    position: new GDMAP.value.AMap.LngLat(121.495000, 31.233366), // 必须
    scale: 800, // 非必须，默认1
    height: -100, // 非必须，默认0
    scene: 0, // 非必须，默认0
  }

  const gltfObj = new GDMAP.value.AMap.GltfLoader()

  // gltfObj.load(urlCity, function (gltfCity) {
  //   cityMeshes = gltfCity;
  //   gltfCity.setOption(paramCity);
  //   gltfCity.rotateX(90);
  //   gltfCity.rotateZ(120);
  //   object3Dlayer.add(gltfCity);
  // });

  gltfObj.load(urlDuck, (gltfDuck) => {
    druckMeshes = gltfDuck
    gltfDuck.setOption(paramDuck)
    gltfDuck.rotateX(90)
    gltfDuck.rotateZ(-20)
    object3Dlayer.add(gltfDuck)
  })
})
</script>

<template>
  <div class="relative">
    <div id="map-container" ref="mapContainerRef" />
    <div class="fixed top-5 left-5 bg-gray-100 w-[80px] text-center p-2 rounded-xl border border-amber-300">
      {{ currentZoom.toFixed(3) }}
    </div>
    <el-button class="fixed top-2 right-2" @click="handleShowCustomMap">
      制图
    </el-button>
    <div
      class="fixed top-[300px] right-2 px-1 py-1
             bg-orange-300 rounded-xl"
    >
      <el-popover
        placement="left"
        trigger="click"
      >
        <div class="flex max-w-[100px] overflow-scroll">
          <img
            v-for="index in 3"
            :key="index" class="h-[50px]"
            :src="`/static/img/maker-${index}.png`" alt=""
            draggable="true"
            @dragend="handleImgDragEnd"
          >
        </div>
        <template #reference>
          <button class="px-2 py-1 rounded-xl">
            自定义
          </button>
        </template>
      </el-popover>
    </div>
    <div
      class="fixed top-[500px] right-2 px-1 py-1
             bg-orange-300 rounded-xl"
    >
      <el-popover
        placement="left"
        trigger="click"
      >
        <div class="flex max-w-[100px] overflow-scroll">
          <div
            class="border border-amber-200"
            draggable="true"
            @dragend="handleTextDragEnd"
          >
            拖拽添加文字
          </div>
        </div>
        <template #reference>
          <button class="px-2 py-1 rounded-xl">
            自定义文字
          </button>
        </template>
      </el-popover>
    </div>
    <div v-show="fabricCanvasVisible" class="fixed top-0 left-0">
      <canvas id="fabric-canvas" />
    </div>
  </div>
</template>

<style scoped>
  #map-container {
    padding: 0;
    margin: 0;
    width: 100vw;
    height: 100vh;
  }
</style>
