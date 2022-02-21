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
            v-model="queryIp"
            type="text"
            class="flex-1 py-3 px-4 rounded-tl-md rounded-bl-md focus:outline-none"
            placeholder="Search for any IP address"
            @keyup.enter="getIpInfo"
          />
          <i
            class="cursor-pointer bg-blue-400 text-white px-4 rounded-tr-md rounded-br-md flex items-center fa-solid fa-chevron-right"
            @click="getIpInfo"
          ></i>
        </div>
      </div>
      <!-- IP Info -->
      <IPInfo v-if="ipInfo" :ipInfo="ipInfo" />
    </div>
    <!-- Map -->
    <div id="mapid" class="h-full z-10"></div>
  </div>
</template>

<script>
// @ is an alias to /src
import IPInfo from "@/components/IPInfo.vue";
import leaflet from "leaflet";
import { onMounted, ref } from "vue";
import axios from "axios";

export default {
  name: "Home",
  components: { IPInfo },
  setup() {
    let mymap;
    const queryIp = ref("");
    const ipInfo = ref(null);

    onMounted(() => {
      mymap = leaflet.map("mapid").setView([37.5445, 126.9837], 13);

      leaflet
        .tileLayer(
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
        )
        .addTo(mymap);
    });

    const getIpInfo = async () => {
      try {
        const data = await axios.get(
          `https://geo.ipify.org/api/v2/country,city,vpn?apiKey=at_YrmWNvQJO8BTBqJ3YfagaEeHnB3QJ&ipAddress=${queryIp.value}`
        );
        const result = data.data;
        console.log(result);
        ipInfo.value = {
          address: result.ip,
          state: result.location.region,
          timezone: result.location.timezone,
          isp: result.isp,
          lat: result.location.lat,
          lng: result.location.lng,
        };
        leaflet.marker([ipInfo.value.lat, ipInfo.value.lng]).addTo(mymap);
        mymap.setView([ipInfo.value.lat, ipInfo.value.lng], 13);
      } catch (err) {
        alert(err.message);
      }
    };

    return { queryIp, ipInfo, getIpInfo };
  },
};
</script>
