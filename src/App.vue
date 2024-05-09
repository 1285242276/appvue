<template>
  <div id="map"></div>
</template>

<script>
import Map from 'ol/Map'
import View from 'ol/View'
import TileLayer from 'ol/layer/Tile'
import OSM from 'ol/source/OSM'
import VectorLayer from 'ol/layer/Vector'
import VectorSource from 'ol/source/Vector'
import { LineString } from 'ol/geom'
import Feature from 'ol/Feature'
import Style from 'ol/style/Style'
import Text from 'ol/style/Text'
import Fill from 'ol/style/Fill'
import Stroke from 'ol/style/Stroke'

export default {
  name: 'App',
  data() {
    return {
      msg: 'Welcome to Your Vue.js App',
    }
  },
  mounted() {
    const map = new Map({
      target: 'map',
      layers: [
        new TileLayer({
          source: new OSM(),
        }),
      ],
      view: new View({
        center: [0, 0],
        zoom: 2,
        // projection: 'EPSG:4326',
      }),
    })

    const line = new LineString([
      [0, 0],
      [5000000, 5000000],
    ])

    var lineFeature = new Feature({
      geometry: line,
    })

    var textStyle = new Text({
      text: 'My Line111',
      font: '12px Calibri,sans-serif',
      fill: new Fill({ color: '#000' }),
      stroke: new Stroke({ color: '#f00', width: 2 }),
      offsetX: 0,
      offsetY: -20,
      textAlign: 'center',
    })

    var lineStyle = new Style({
      stroke: new Stroke({
        color: 'blue',
        width: 2,
      }),
      text: textStyle,
    })

    lineFeature.setStyle(lineStyle)

    const vectorLayer = new VectorLayer({
      source: new VectorSource({
        features: [lineFeature],
      }),
    })

    map.addLayer(vectorLayer)

    // 监听地图的缩放事件
    map.getView().on('change:resolution', function () {
      // 获取当前的缩放等级
      const zoomLevel = map.getView().getZoom()

      // 获取地图投影的单位（通常是米）
      const projection = map.getView().getProjection()
      // const units = projection.getUnits()

      // 获取当前视图的分辨率（单位是地图单位/像素）
      const resolution = map.getView().getResolution()

      // 获取地图投影的单位对应的米数
      const metersPerUnit = projection.getMetersPerUnit()

      // 计算比例尺
      const scale = resolution * metersPerUnit

      console.log('当前缩放等级:', zoomLevel)
      console.log('比例尺:', scale)
    })
  },
}
</script>

<style>
#map {
  width: 100vw;
  height: 100vh;
}
</style>
