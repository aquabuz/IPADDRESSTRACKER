<template>
  <div class="flex flex-col h-screen max-h-screen">
    <!-- Search / Results -->
    <div
      class="flex justify-center relative bg-blue-200 bg-cover px-4 pt-8 pb-32 z-20"
    >
      <!-- Search Input -->
      <div class="w-full max-w-screen-sm">
        <h1 class="text-blue-400 text-center text-3xl pb-4">
          Ip Address Tracker
        </h1>
        <div class="flex">
          <input
            type="text"
            class="flex-1 py-3 px-4 rounded-tl-md rounded-bl-md focus:outline-none"
            placeholder="Search for any IP address"
          />
          <i
            class="cursor-pointer bg-blue-400 text-white px-4 rounded-tr-md rounded-br-md flex items-center fa-solid fa-chevron-right"
          ></i>
        </div>
      </div>
      <!-- IP Info -->
      <IPInfo />
    </div>
    <!-- Map -->
    <div id="map" class="h-full z-10"></div>
  </div>
</template>

<script>
// @ is an alias to /src
import IPInfo from "@/components/IPInfo.vue";
import { map, tileLayer } from "leaflet";
import { onBeforeUnmount, onMounted } from "vue";

export default {
  name: "Home",
  components: { IPInfo },
  setup() {
    let mymap;

    onMounted(() => {
      mymap = map("map").setView([51.505, -0.09], 13);

      tileLayer(
        "https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=pk.eyJ1IjoiYXF1YWJ1eiIsImEiOiJja3p3b3U5MjEyam9yMnBuYWZrZHpybnB2In0.q3DIjcD4FGSpi990q97W4A",
        {
          attribution:
            'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
          maxZoom: 18,
          id: "mapbox/streets-v11",
          tileSize: 512,
          zoomOffset: -1,
          accessToken:
            "pk.eyJ1IjoiYXF1YWJ1eiIsImEiOiJja3p3b3U5MjEyam9yMnBuYWZrZHpybnB2In0.q3DIjcD4FGSpi990q97W4A",
        }
      ).addTo(mymap);
    });

    onBeforeUnmount(() => {
      mymap.remove();
    });
  },
};
</script>
