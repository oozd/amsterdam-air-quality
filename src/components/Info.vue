<template>
  <v-card width="270px">
    <v-list-item>
      <v-list-item-content class="font-weight-bold text-center">
        <v-list-item-title>
          {{ props.station.city.name }}
        </v-list-item-title>
        <v-list-item-subtitle>
          <v-divider class="dark ma-2" />
          Average:
          {{ props.station.forecast.daily.pm25[2].avg }}</v-list-item-subtitle
        >
        <v-list-item-subtitle
          >Min:
          {{ props.station.forecast.daily.pm25[2].min }}</v-list-item-subtitle
        >
        <v-list-item-subtitle
          >Max:
          {{ props.station.forecast.daily.pm25[2].max }}</v-list-item-subtitle
        >
      </v-list-item-content>
    </v-list-item>
  </v-card>
</template>

<script lang="ts">
export default {
  name: "Info",
  props: {
    station: {
      type: Object,
      required: true,
    },
  },
  setup(props) {
    return {
      props,
    };
  },
};
</script>
