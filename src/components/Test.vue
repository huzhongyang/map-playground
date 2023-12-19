<script setup  lang="ts">
import AMapLoader from '@amap/amap-jsapi-loader'
import { onMounted, ref, shallowRef } from 'vue'
import * as THREE from 'three'
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js'

const defaultValue = {
  SURFACE: {
    searchValue: null,
    createBy: '',
    createTime: '2023-10-05 08:53:24',
    updateBy: '',
    updateTime: '2023-09-28 18:09:05',
    remark: null,
    params: {},
    id: 783,
    useId: '231',
    useType: 'USER',
    name: '商圈板块',
    mapId: null,
    surfaceType: 'SURFACE',
    borderColor: '#ffffff',
    borderWidth: 1,
    fillColor: 'rgba(161,218,161, 0.6)',
    fontSize: 12,
    fontColor: '#ffffff',
    titleBgColor: 'rgba(161,218,161, 1)',
    titleBgHeight: 0,
    transparency: null,
    drawMapId: '233',
    areaId: null,
    minZoomLevel: 8,
    maxZoomLevel: 11,
    sortNum: '1',
    styleType: 'BUILDING',
  },
  NEW_HOUSE: {
    searchValue: null,
    createBy: '',
    createTime: '2023-10-05 08:53:24',
    updateBy: '',
    updateTime: '2023-09-28 18:21:31',
    remark: null,
    params: {},
    id: 784,
    useId: '231',
    useType: 'USER',
    name: '新房',
    mapId: null,
    surfaceType: 'NEW_HOUSE',
    borderColor: '#ffffff',
    borderWidth: 1,
    fillColor: '#F36923',
    fontSize: 12,
    fontColor: '#ffffff',
    titleBgColor: '#F36923',
    titleBgHeight: 0,
    transparency: null,
    drawMapId: '233',
    areaId: null,
    minZoomLevel: 12,
    maxZoomLevel: 17,
    sortNum: '1',
    styleType: 'BUILDING',
  },
  SECOND_HAND_HOUSE: {
    searchValue: null,
    createBy: '',
    createTime: '2023-10-05 08:53:24',
    updateBy: '',
    updateTime: '2023-09-28 18:09:05',
    remark: null,
    params: {},
    id: 785,
    useId: '231',
    useType: 'USER',
    name: '二手房',
    mapId: null,
    surfaceType: 'SECOND_HAND_HOUSE',
    borderColor: '#ffffff',
    borderWidth: 1,
    fillColor: 'rgba(255,255,2,0.6)',
    fontSize: 12,
    fontColor: '#F36923',
    titleBgColor: 'rgba(255,255,2,1)',
    titleBgHeight: 0,
    transparency: null,
    drawMapId: '233',
    areaId: null,
    minZoomLevel: 12,
    maxZoomLevel: 17,
    sortNum: '2',
    styleType: 'BUILDING',
  },
  SCHOOL: {
    searchValue: null,
    createBy: '',
    createTime: '2023-10-05 08:53:24',
    updateBy: '',
    updateTime: '2023-09-28 18:09:05',
    remark: null,
    params: {},
    id: 786,
    useId: '231',
    useType: 'USER',
    name: '学校',
    mapId: null,
    surfaceType: 'SCHOOL',
    borderColor: '#ffffff',
    borderWidth: 1,
    fillColor: 'rgba(255,127,255,0.6)',
    fontSize: 12,
    fontColor: '#ffffff',
    titleBgColor: 'rgba(255,127,255,1)',
    titleBgHeight: 0,
    transparency: null,
    drawMapId: '233',
    areaId: null,
    minZoomLevel: 12,
    maxZoomLevel: 17,
    sortNum: '3',
    styleType: 'BUILDING',
  },
  HOUSE: {
    searchValue: null,
    createBy: '',
    createTime: '2023-10-05 08:53:24',
    updateBy: '',
    updateTime: '2023-09-28 18:09:05',
    remark: null,
    params: {},
    id: 787,
    useId: '231',
    useType: 'USER',
    name: '土拍',
    mapId: null,
    surfaceType: 'HOUSE',
    borderColor: '#ffffff',
    borderWidth: 1,
    fillColor: 'rgba(255,0,127, 0.6)',
    fontSize: 12,
    fontColor: '#ffffff',
    titleBgColor: 'rgba(255,0,127, 1)',
    titleBgHeight: 0,
    transparency: null,
    drawMapId: '233',
    areaId: null,
    minZoomLevel: 12,
    maxZoomLevel: 17,
    sortNum: '4',
    styleType: 'BUILDING',
  },
  GOVERNMENT: {
    searchValue: null,
    createBy: '',
    createTime: '2023-10-05 08:53:24',
    updateBy: '',
    updateTime: '2023-09-28 18:09:05',
    remark: null,
    params: {},
    id: 789,
    useId: '231',
    useType: 'USER',
    name: '政府机构',
    mapId: null,
    surfaceType: 'GOVERNMENT',
    borderColor: '#ffffff',
    borderWidth: 1,
    fillColor: 'rgba(137,87,178,0.6)',
    fontSize: 12,
    fontColor: '#ffffff',
    titleBgColor: 'rgba(137,87,178,1)',
    titleBgHeight: 0,
    transparency: null,
    drawMapId: '233',
    areaId: null,
    minZoomLevel: 12,
    maxZoomLevel: 17,
    sortNum: '6',
    styleType: 'PUBLICFACILITIES',
  },
  BUSINESS: {
    searchValue: null,
    createBy: '',
    createTime: '2023-10-05 08:53:24',
    updateBy: '',
    updateTime: '2023-09-28 18:09:05',
    remark: null,
    params: {},
    id: 790,
    useId: '231',
    useType: 'USER',
    name: '商业',
    mapId: null,
    surfaceType: 'BUSINESS',
    borderColor: '#ffffff',
    borderWidth: 1,
    fillColor: 'rgba(255,0,0,0.6)',
    fontSize: 12,
    fontColor: '#ffffff',
    titleBgColor: 'rgba(255,0,0,1)',
    titleBgHeight: 0,
    transparency: null,
    drawMapId: '233',
    areaId: null,
    minZoomLevel: 12,
    maxZoomLevel: 17,
    sortNum: '7',
    styleType: 'PUBLICFACILITIES',
  },
  PARK: {
    searchValue: null,
    createBy: '',
    createTime: '2023-10-05 08:53:24',
    updateBy: '',
    updateTime: '2023-09-28 18:09:05',
    remark: null,
    params: {},
    id: 791,
    useId: '231',
    useType: 'USER',
    name: '公园景区',
    mapId: null,
    surfaceType: 'PARK',
    borderColor: '#ffffff',
    borderWidth: 1,
    fillColor: 'rgba(0,245,2,0.6)',
    fontSize: 12,
    fontColor: '#ffffff',
    titleBgColor: 'rgba(0,245,2,1)',
    titleBgHeight: 0,
    transparency: null,
    drawMapId: '233',
    areaId: null,
    minZoomLevel: 12,
    maxZoomLevel: 17,
    sortNum: '8',
    styleType: 'PUBLICFACILITIES',
  },
  HOSPITAL: {
    searchValue: null,
    createBy: '',
    createTime: '2023-10-05 08:53:24',
    updateBy: '',
    updateTime: '2023-09-28 18:09:05',
    remark: null,
    params: {},
    id: 792,
    useId: '231',
    useType: 'USER',
    name: '医院',
    mapId: null,
    surfaceType: 'HOSPITAL',
    borderColor: '#ffffff',
    borderWidth: 1,
    fillColor: 'rgba(255,0,127, 0.6)',
    fontSize: 12,
    fontColor: '#ffffff',
    titleBgColor: 'rgba(255,0,127, 1)',
    titleBgHeight: 0,
    transparency: null,
    drawMapId: '233',
    areaId: null,
    minZoomLevel: 8,
    maxZoomLevel: 11,
    sortNum: '9',
    styleType: 'PUBLICFACILITIES',
  },
}
const mapContainerRef = ref<HTMLDivElement>()
const GDMAP = shallowRef<{
  AMap: typeof AMap
  map: typeof AMap.Map
  loca: typeof Loca
}>({
  AMap: {},
  map: {},
  loca: {},
})
let camera: THREE.PerspectiveCamera
let render: THREE.WebGLRenderer
let scene: THREE.Scene
let dirLight: THREE.DirectionalLight

function initScene() {
  // 初始化场景
  scene = new THREE.Scene()
  scene.castShadow = true
}

// 初始化光源
function initLight() {
  dirLight = new THREE.DirectionalLight('white', 2)
  dirLight.position.set(0, 200, 200)
  scene.add(dirLight)
  // 设置用于计算阴影的光源对象
  dirLight.castShadow = true
  // 设置计算阴影的区域，最好刚好紧密包围在对象周围
  // 计算阴影的区域过大：模糊  过小：看不到或显示不完整
  dirLight.shadow.camera.near = 0.5
  dirLight.shadow.camera.far = 300
  dirLight.shadow.camera.left = -50
  dirLight.shadow.camera.right = 50
  dirLight.shadow.camera.top = 200
  dirLight.shadow.camera.bottom = -100
  // 设置mapSize属性可以使阴影更清晰，不那么模糊
  dirLight.shadow.mapSize.set(2048, 2048)
}

// 初始化相机
function initCamera() {
  camera = new THREE.PerspectiveCamera(
    60,
    mapContainerRef.value!.clientWidth / mapContainerRef.value!.clientHeight,
    100,
    1 << 30,
  )
}
let customCoords: any
let comLocation: any

// 声明raycaster和mouse变量
let raycaster: THREE.Raycaster
let mouse: THREE.Vector2

onMounted(async () => {
  await initMap()

  raycaster = new THREE.Raycaster()
  mouse = new THREE.Vector2()

  // 初始化数据转换工具
  customCoords = GDMAP.value.map.customCoords
  // 数据使用转换工具进行转换，这个操作必须要提前执行（在获取镜头参数 函数之前执行），否则将会获得一个错误信息。
  comLocation = customCoords.lngLatsToCoords([117.318631, 31.866352])

  const glLayer = new GDMAP.value.AMap.GLCustomLayer({
    zIndex: 10,
    init: (gl: any) => {
      render = new THREE.WebGLRenderer({
        context: gl, // 地图的 gl 上下文
      })
      // 自动清空画布这里必须设置为 false，否则地图底图将无法显示
      render.autoClear = false
      render.outputEncoding = THREE.SRGBColorSpace
      // render.shadowMap.enabled = true
      render.shadowMap.type = THREE.PCFSoftShadowMap
      // 初始化场景
      initScene()
      // 初始化光源
      initLight()
      // 初始化相机
      initCamera()

      window.addEventListener('resize', () => {
        render.setSize(mapContainerRef.value!.clientWidth, mapContainerRef.value!.clientHeight)
        camera.aspect = mapContainerRef.value!.clientWidth / mapContainerRef.value!.clientHeight
        camera.updateProjectionMatrix()
      })

      window.addEventListener('click', onMouseClick, false)

      // Load a glTF resource
      const loader = new GLTFLoader()
      loader.load(
        // resource URL
        'https://shanghai-house-model.oss-accelerate.aliyuncs.com/glb/中国铁建花语天境/ee3605fb-18d6-42ae-b278-4e266704e9a0.glb',
        // called when the resource is loaded
        (gltf: any) => {
          // gltf.scene.castShadow = true
          gltf.scene.traverse((child: any) => {
            // 底部线隐藏
            console.log(child)
            if (child.name.includes('Line'))
              child.visible = false
            if (child.isMesh) {
              // 底部隐藏
              if (child.name.includes('001') || child.name === '??_1')
                child.visible = false
              child.material.emissive = child.material.color
              child.material.emissiveMap = child.material.map
              // 开启阴影 (过滤楼号)
              child.castShadow = !child.name.includes('Rectangle')
              child.receiveShadow = true
              child.frustumCulled = false
            }
          })
          gltf.scene.rotation.x = 0.5 * Math.PI
          gltf.scene.scale.set(1.2, 1.2, 1.2)

          // const light = new THREE.PointLight(0xFFCCAA, 1, 100) // 使用较低的色温，如橙色
          // light.position.set(0, 0, 10)
          // scene.add(light)
          //
          // const material = new THREE.MeshLambertMaterial({ color: 0xFFCCAA }) // 使用与光源相同的颜色
          // const mesh = new THREE.Mesh(new THREE.BufferGeometry(), material)
          // scene.add(mesh)
          //
          // const light2 = new THREE.PointLight(0xFF0000, 1, 100) // 使用较高饱和度的颜色，如红色
          // light.position.set(0, 0, 10)
          // scene.add(light2)
          //
          // const material2 = new THREE.MeshLambertMaterial({ color: 0xFF0000 }) // 使用与光源相同的颜色
          // const mesh2 = new THREE.Mesh(new THREE.BufferGeometry(), material2)
          // scene.add(mesh2)

          scene.add(gltf.scene)

          gltf.animations // Array<THREE.AnimationClip>
          gltf.scene // THREE.Group
          gltf.scenes // Array<THREE.Group>
          gltf.cameras // Array<THREE.Camera>
          gltf.asset // Object
        },
        // called while loading is progressing
        (xhr) => {
          console.log(`${xhr.loaded / xhr.total * 100}% loaded`)
        },
        // called when loading has errors
        (error) => {
          console.log('An error happened')
        },
      )
    },
    render: () => {
      render.resetState()
      customCoords.setCenter([117.318631, 31.866352])
      const { near, far, fov, up, lookAt, position } = customCoords.getCameraParams()
      // 这里的顺序不能颠倒，否则可能会出现绘制卡顿的效果。
      camera.near = near
      camera.far = far
      camera.fov = fov
      camera.position.set(...position)
      camera.up.set(...up)
      camera.lookAt(...lookAt)
      camera.updateProjectionMatrix()
      const time = Date.now() * 0.0005
      dirLight.position.x = Math.cos(time) * 0.75 + dirLight.position.x
      dirLight.position.y = Math.sin(time) * 0.75 + dirLight.position.y
      dirLight.position.z = Math.sin(time) * 0.1 + dirLight.position.z
      render.render(scene, camera)
      render.resetState()
    },
  })
  console.log(glLayer)
  GDMAP.value.map.add(glLayer)

  // await renderBoundAndLabel()
})

async function onMouseClick(event: MouseEvent) {
  // 通过鼠标点击的位置计算出raycaster所需要的点的位置，以屏幕中心为原点，值的范围为-1到1.
  mouse.x = (event.clientX / window.innerWidth) * 2 - 1
  mouse.y = -(event.clientY / window.innerHeight) * 2 + 1

  // 通过鼠标点的位置和当前相机的矩阵计算出raycaster
  raycaster.setFromCamera(mouse, camera)

  // 获取raycaster直线和所有模型相交的数组集合
  const intersects = raycaster.intersectObjects(scene.children)
  console.log(intersects[0])

  const randomColor = `#${Math.floor(Math.random() * 16777215).toString(16)}`
  // 将所有的相交的模型的颜色设置为红色，如果只需要将第一个触发事件，那就数组的第一个模型改变颜色即可
  // for (let i = 0; i < intersects.length; i++)
  intersects[0]?.object.material.color.set(randomColor)
  render.render(scene, camera)
}

async function initMap() {
  const AMap = await AMapLoader.load({
    key: 'dc86960982b43599bb32467299efb250',
    version: '2.0',
    plugins: ['AMap.ElasticMarker', 'AMap.CustomLayer'],
    Loca: { // 是否加载 Loca， 缺省不加载
      version: '2.0.0', // Loca 版本，缺省 1.3.2
    },
  })
  GDMAP.value.AMap = AMap
  GDMAP.value.map = new AMap.Map(mapContainerRef.value as HTMLDivElement, {
    zoom: 10,
    zooms: [8, 20],
    center: [117.318631, 31.866352],
    viewMode: '3D',
    features: ['bg', 'road'],
  })
  GDMAP.value.loca = new Loca.Container({
    map: GDMAP.value.map,
  })

  GDMAP.value.map.on('zoomend', () => {
    console.log(GDMAP.value.map.getZoom())
  })
  GDMAP.value.map.on('dragend', () => {
    console.log('触发停止拖拽地图时的事件')
  })
}

// 控规 标签
async function renderBoundAndLabel() {
  const data = await (await fetch(new URL('/static/bounds.json', import.meta.url).href)).json()

  const geoJson = toGeoJson(data)
  console.log(geoJson)
  const loca = new Loca.Container({
    map: GDMAP.value.map,
  })
  const pl = new Loca.PolygonLayer({
    loca,
    zoom: [2, 20],
    visible: true,
    hasSide: false,
    depth: false,
    zIndex: 120,
    opacity: 1,
  })
  const dataSource = new Loca.GeoJSONSource({
    data: geoJson,
  })
  pl.setSource(dataSource)
  pl.setStyle({
    height: 0,
    topColor: (_, feature) => {
      return defaultValue[feature.properties.surfaceType]?.fillColor || 'rgba(255,0,127, 0.6)'
    },
  })
  loca.add(pl)

  // 文本标签
  const textStyle = {
    fontSize: 10,
    fontWeight: 'normal',
    fillColor: '#22886f',
    strokeColor: '#fff',
    strokeWidth: 2,
    fold: true,
  }
  const labelData = []
  console.log(data)

  let index = 0
  for (const d of data) {
    if (index > 5000)
      continue
    index += 1
    labelData.push(new GDMAP.value.AMap.LabelMarker({
      position: [d.lon, d.lat],
      zooms: [10, 20],
      opacity: 1,
      zIndex: 23,
      text: {
        content: d.name,
        style: textStyle,
      },
      // icon: {
      //   type: 'image',
      //   image: 'https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png',
      //   size: [6, 9],
      //   anchor: 'bottom-center',
      // },
    }))
  }
  const labelsLayer = new GDMAP.value.AMap.LabelsLayer({
    zooms: [3, 20],
    zIndex: 1000,
    collision: true,
    // 设置 allowCollision：true，可以让标注避让用户的标注
    allowCollision: true,
  })
  labelsLayer.add(labelData)
  // 图层添加到地图
  GDMAP.value.map.add(labelsLayer)

  // const polygons = []
  // for (const d of data) {
  //   const polygonArr = JSON.parse(d.polygon).coordinates[0]
  //   polygons.push(new AMap.Polygon({
  //     path: polygonArr, // 设置多边形边界路径
  //     strokeColor: '#FF33FF', // 线颜色
  //     strokeOpacity: 0.2, // 线透明度
  //     strokeWeight: 3, // 线宽
  //     fillColor: '#1791fc', // 填充色
  //     fillOpacity: 0.35, // 填充透明度
  //   }))
  // }
  // const overlayGroup = new AMap.OverlayGroup(polygons)
  // GDMAP.value.map.add(overlayGroup)
}

function toGeoJson(data: []) {
  const geoJson = {
    type: 'FeatureCollection',
    features: [],
  }
  for (const d of data) {
    const polygonArr = JSON.parse(d.polygon).coordinates
    const feature = {
      type: 'Feature',
      properties: {
        name: d.name,
        id: d.id,
        surfaceType: d.surfaceType,
      },
      geometry: {
        type: 'Polygon',
        coordinates: polygonArr,
      },
    }
    geoJson.features.push(feature)
  }
  return geoJson
}

function rgbToHex(rgb: string) {
  // 将 RGB 或 RGBA 字符串转换为数组
  const rgbArray = rgb.replace(/[^\d,]/g, '').split(',')

  // 提取 RGB 值
  const red = Number.parseInt(rgbArray[0])
  const green = Number.parseInt(rgbArray[1])
  const blue = Number.parseInt(rgbArray[2])

  // 提取透明度（如果存在）
  let alpha = 1
  if (rgbArray.length === 4)
    alpha = Number.parseFloat(rgbArray[3])

  // 将 RGB 值转换为十六进制
  const hex = ((red << 16) | (green << 8) | blue).toString(16).padStart(6, '0')

  // 返回十六进制和透明度
  return { hex: `#${hex}`, alpha }
}
</script>

<template>
  <div id="map-container" ref="mapContainerRef" />
</template>

<style scoped>
  #map-container {
    padding: 0;
    margin: 0;
    width: 100vw;
    height: 100vh;
  }
</style>
