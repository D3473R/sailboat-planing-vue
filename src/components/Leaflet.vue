<template>
  <div class="container">
    <div class="info">
      <p>Center: {{ center }}</p>
      <p>Zoom: {{ zoom }}</p>
      <p>Bounds: {{ bounds }}</p>
    </div>
    <div class="map">
      <l-map
        :zoom="zoom"
        :center="center"
        @update:zoom="zoomUpdated"
        @update:center="centerUpdated"
        @update:bounds="boundsUpdated"
        @click="handleMapClick"
      >
        <l-tile-layer :url="url"></l-tile-layer>
        <l-marker v-for="item in markers" :key="item.id" :lat-lng="item.latlng" :draggable="true" @click="handleMarkerClick" @dragend="handleMarkerDragend($event, item.id)">
          <l-popup>
            <div v-on:click="handleTrashClick($event, item.id)">
              <unicon class="icon" name="trash-alt" fill="royalblue"></unicon>
            </div>
            </l-popup>
        </l-marker>
        <l-polyline
          :lat-lngs="polyline.latlngs"
          :color="polyline.color"
        >
        </l-polyline>
      </l-map>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import { LMap, LTileLayer, LPolyline, LMarker, LPopup } from 'vue2-leaflet'

Vue.component('l-map', LMap)
Vue.component('l-tile-layer', LTileLayer)
Vue.component('l-polyline', LPolyline)
Vue.component('l-marker', LMarker)
Vue.component('l-popup', LPopup)

export default {
  data () {
    return {
      url: 'https://{s}.tile.osm.org/{z}/{x}/{y}.png',
      zoom: 14,
      center: [60.103282, 19.96425],
      bounds: null,
      polyline: {
        uuids: [],
        latlngs: [],
        color: 'green'
      },
      markers: []
    };
  },
  methods: {
    zoomUpdated (zoom) {
      this.zoom = zoom;
    },
    centerUpdated (center) {
      this.center = center;
    },
    boundsUpdated (bounds) {
      this.bounds = bounds;
    },
    handleMapClick(event) {
      const uuid = this.$uuid.v4();
      this.markers.push({
        id: uuid,
        latlng: event.latlng,
        content: 'Another'
      });
      this.polyline.uuids.push(uuid);
      this.polyline.latlngs.push(event.latlng);
    },
    handleMarkerClick(event) { // eslint-disable-line no-unused-vars
    },
    handleMarkerDragend(event, id) {
      const index = this.polyline.uuids.indexOf(id);
      Vue.set(this.polyline.latlngs, index, event.target._latlng)
    },
    handleTrashClick(event, id) {
      const index = this.polyline.uuids.indexOf(id);
      this.polyline.latlngs.splice(index, 1);
      this.polyline.uuids.splice(index, 1);
      this.markers.splice(index, 1);
    }
  }
}
</script>

<style>
@import "~leaflet/dist/leaflet.css";

.container {
  width: 100%;
  height: 100%;
  display: grid;
  grid-template-columns: 10% auto 10%;
  grid-template-rows: 20% auto 5%;
}

.info {
  grid-column-start: 2;
  grid-row-start: 1;
}

.map {
  grid-column-start: 2;
  grid-row-start: 2;
  width: 100%;
  height: 100%;
}

.icon {
  cursor: pointer;
}
</style>
