<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <label for="search" class="hidden">Search</label>
      <input
        id="search"
        type="text"
        v-model="searchQuery"
        @input="getSearchResults"
        placeholder="Search for the city or state"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0#004e71]"
      />
      <ul
        class="absolute bg-weather-secondary text-white w-full shadow-md py-2 px-1 top-[66px]"
        v-if="mapboxSearchResults"
      >
        <li
          v-for="searchResult in mapboxSearchResults"
          :key="searchResult.id"
          class="py-2 cursor-pointer"
        >
          {{ searchResult.properties.place_formatted }}
        </li>
      </ul>
    </div>
  </main>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";

const apiToken =
  "pk.eyJ1IjoiaWZhMzAwMCIsImEiOiJjbHY2eGJpeXYwNGRhMnFvYWZ3djczdXloIn0.KcuQk5IZF4iLmR6TX4-TmA";
const searchQuery = ref("");
const queryTimeout = ref(null);
const mapboxSearchResults = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      const res = await axios.get(
        `https://api.mapbox.com/search/geocode/v6/forward?q=${searchQuery.value}&access_token=${apiToken}&types=place`
      );
      mapboxSearchResults.value = res.data.features;
      return;
    }
    mapboxSearchResults.value = null;
  }, 300);
};
</script>
