<template>
  <v-app>
    <v-app-bar app color="white">
      <v-img
        alt="Logo"
        class="shrink mr-2 mb-2"
        src="@/assets/app-bar-logo.png"
        width="80"
      />
      <v-spacer />
      <v-toolbar-title>Amsterdam</v-toolbar-title>
      <v-spacer />
      <v-switch
        v-model="switchState"
        hide-details="true"
        label="teal"
        color="teal"
      >
        <template v-slot:label>
          {{ switchLabel }}
          <v-progress-circular
            :indeterminate="switchState"
            size="30"
            class="ml-2"
            color="teal"
          ></v-progress-circular>
        </template>
      </v-switch>
    </v-app-bar>
    <v-main>
      <Map :stations="stationFeeds" :stationLocations="stationLocations" />
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from "vue";
import Map from "./components/Map.vue";
import { getStations, getStationFeeds, AMSTERDAM_BOUNDS } from "./helpers";
import VueCompositionAPI, {
  onBeforeMount,
  Ref,
  ref,
  watch,
} from "@vue/composition-api";

Vue.use(VueCompositionAPI);

export default Vue.extend({
  name: "App",
  components: {
    Map,
  },
  setup() {
    const stationLocations: Ref<Array<Record<string, number>>> = ref([]);
    let stations: [] = [];
    const stationFeeds: Ref<Array<Record<string, any>>> = ref([]);
    const switchState: Ref<boolean> = ref(false);
    const switchLabel = "Click to start/stop auto refresh";
    let intervalId: number;
    onBeforeMount(async () => {
      const response = await getStations(AMSTERDAM_BOUNDS);
      stationLocations.value = response.locations;
      stations = response.stations;
      stationFeeds.value = await getStationFeeds(stations);
    });
    watch(switchState, refreshData);
    async function refreshData() {
      if (switchState.value === false) {
        clearInterval(intervalId);
        return;
      }
      intervalId = setInterval(async () => {
        stationFeeds.value = await getStationFeeds(stations);
      }, 10000);
    }
    return {
      stationFeeds,
      stationLocations,
      switchLabel,
      switchState,
    };
  },
});
</script>
