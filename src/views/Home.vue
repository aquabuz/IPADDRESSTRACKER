<template>
  <div class="flex flex-col h-screen max-h-screen">
    <div
      class="flex justify-center relative bg-blue-200 bg-cover px-4 pt-8 pb-32"
    >
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
      <IPInfo />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import IPInfo from "@/components/IPInfo.vue";

export default {
  name: "Home",
  components: { IPInfo },
};
</script>
