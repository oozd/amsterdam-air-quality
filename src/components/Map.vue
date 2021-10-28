<template>
  <div class="fill-height">
    <gmap-map id="map" :zoom="12" :center="center">
      <GmapMarker
        v-for="(station, index) in stations"
        :key="station.idx"
        :position="stationLocations[index]"
        @mouseover="showByIndex = station.idx"
        @mouseout="showByIndex = null"
        :icon="iconGenerator(station.forecast.daily.pm25[2].avg)"
      >
        <gmap-info-window :opened="showByIndex === station.idx">
          <info :station="station" />
        </gmap-info-window>
      </GmapMarker>
    </gmap-map>
  </div>
</template>

<script lang="ts">
import { Ref, ref } from "@vue/composition-api";
import { iconGenerator, AMSTERDAM_CENTER_LAT_LNG } from "./../helpers";
import Info from "./Info.vue";
export default {
  name: "Map",
  components: {
    Info,
  },
  props: {
    stations: {
      type: Array,
      required: true,
    },
    stationLocations: {
      type: Array,
      required: true,
    },
  },
  setup(props) {
    const showByIndex: Ref<number> = ref(0);
    return {
      center: {
        lat: AMSTERDAM_CENTER_LAT_LNG[0],
        lng: AMSTERDAM_CENTER_LAT_LNG[1],
      },
      props,
      showByIndex,
      iconGenerator,
    };
  },
};
</script>

<style>
button.gm-ui-hover-effect {
  display: none !important;
}
.vue-map-container {
  width: 100%;
  height: 100%;
}
</style>
