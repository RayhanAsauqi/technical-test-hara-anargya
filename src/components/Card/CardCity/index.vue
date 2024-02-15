<script setup>
import provinces from "../../../assets/js/province.js";
import dataCity from "../../../assets/js/city.js";
import { ref } from "vue";

const provinceId = ref("");
const cityId = ref("");
const provinceName = ref("");

const sortType = ref("id_asc");
const cities = ref([]);
console.log(cities);

const sortedProvinces = ref([]);

const showCities = () => {
  const filteredCities = dataCity.filter(
    (city) => city.provinsi_id === provinceId.value
  );
  cities.value = filteredCities;
};

const showProvince = () => {
  const selectedCity = cities.value.find(
    (city) => city.id === parseInt(cityId.value)
  );
  if (selectedCity) {
    const province = provinces.find(
      (province) => province.id === selectedCity.provinsi_id
    );
    provinceName.value = province ? province.name : "Province Not Found";
  } else {
    provinceName.value = "Province Not Found";
  }
};

const sortProvinces = () => {
  sortedProvinces.value = [...provinces];
  switch (sortType.value) {
    case "id_asc":
      sortedProvinces.value.sort((a, b) => a.id - b.id);
      break;
    case "id_desc":
      sortedProvinces.value.sort((a, b) => b.id - a.id);
      break;
    case "name_asc":
      sortedProvinces.value.sort((a, b) => a.name.localeCompare(b.name));
      break;
    case "name_desc":
      sortedProvinces.value.sort((a, b) => b.name.localeCompare(a.name));
      break;
    default:
      break;
  }
};
</script>

<template>
  <div class="section border rounded-lg shadow-sm">
    <!-- Card City -->
    <div class="px-5 py-6">
      <h2 class="font-extrabold text-lg leading-[30px] text-[#152544]">
        Show Cities by Province ID
      </h2>
      <div class="grid grid-cols-2 gap-2 pt-5 w-1/2">
        <form class="">
          <label
            for="default-search"
            class="mb-2 text-sm font-medium sr-only dark:text-white"
            >Search</label
          >
          <div class="relative">
            <div
              class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none"
            >
              <svg
                class="w-4 h-4 text-gray-500 dark:text-gray-400"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 20 20"
              >
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"
                />
              </svg>
            </div>
            <input
              type="number"
              id="default-search"
              class="block w-full p-4 ps-10 text-sm text-gray-900 border border-gray-300 rounded-lg outline-none"
              placeholder="Input Province ID"
              v-model="provinceId"
            />
          </div>
        </form>

        <button
          @click="showCities"
          class="text-base font-medium leading-[19.36px] text-[#424242] border rounded-lg hover:bg-[#E3F2FD] bg-blue-100 shadow-sm"
        >
          Show Cities
        </button>
      </div>
      <div class="">
        <h2 class="font-extrabold text-md leading-[30px] text-[#152544]">
          City:
        </h2>
        <div class="px-6 py-3 grid grid-cols-2 items-center">
          <ul
            class="text-gray-500 list-disc list-inside dark:text-gray-400"
            v-for="item in cities"
            :key="item.id"
          >
            <li>{{ item.name }} {{ item.id }}</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
  <!-- End Card City -->

  <!-- Card Province -->
  <div class="section border rounded-lg shadow-sm">
    <div class="px-5 py-6">
      <h2 class="font-extrabold text-lg leading-[30px] text-[#152544]">
        Show City by Province ID
      </h2>
      <div class="grid grid-cols-2 gap-2 pt-5 w-1/2">
        <form class="">
          <label
            for="default-search"
            class="mb-2 text-sm font-medium sr-only dark:text-white"
            >Search</label
          >
          <div class="relative">
            <div
              class="absolute inset-y-0 start-0 flex items-center ps-3 pointer-events-none"
            >
              <svg
                class="w-4 h-4 text-gray-500 dark:text-gray-400"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 20 20"
              >
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="m19 19-4-4m0-7A7 7 0 1 1 1 8a7 7 0 0 1 14 0Z"
                />
              </svg>
            </div>
            <input
              type="number"
              id="default-search"
              class="block w-full p-4 ps-10 text-sm text-gray-900 border border-gray-300 rounded-lg outline-none"
              placeholder="Input City ID"
              v-model="cityId"
            />
          </div>
        </form>

        <button
          @click="showProvince"
          class="text-base font-medium leading-[19.36px] text-[#424242] border rounded-lg hover:bg-[#E3F2FD] bg-blue-100 shadow-sm"
        >
          Show Province
        </button>
      </div>
      <div class="">
        <h2 class="font-extrabold text-md leading-[30px] text-[#152544]">
          Province:
        </h2>
        <div class="px-6 py-3 grid grid-cols-2 items-center">
          <ul class="text-gray-500 list-inside dark:text-gray-400">
            <li>{{ provinceName }}</li>
          </ul>
        </div>
      </div>
    </div>
  </div>

  <!-- Card Province end -->

  <!-- Card sorting province -->
  <div class="section border rounded-lg shadow-sm">
    <div class="px-5 py-6">
      <h2>Sort Provinces:</h2>

      <select v-model="sortType" class="border outline-none px-5 py-5">
        <option value="id_asc">ID (Ascending)</option>
        <option value="id_desc">ID (Descending)</option>
        <option value="name_asc">Name (Ascending)</option>
        <option value="name_desc">Name (Descending)</option>
      </select>
      <button
        @click="sortProvinces"
        class="text-base font-medium leading-[19.36px] text-[#424242] border rounded-lg hover:bg-[#E3F2FD] bg-blue-100 shadow-sm"
      >
        Sort
      </button>
      <!-- <table>
        <thead>
          <tr>
            <th>ID</th>
            <th>Name</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="province in sortedProvinces" :key="province.id">
            <td>{{ province.id }}</td>
            <td>{{ province.name }}</td>
          </tr>
        </tbody>
      </table> -->
      <h2 class="font-extrabold text-md leading-[30px] text-[#152544]">
        City:
      </h2>
      <div class="px-10 py-2 grid grid-cols-2 items-center">
        <ul
          class="text-gray-500 list-inside dark:text-gray-400 flex gap-4"
          v-for="item in sortedProvinces"
          :key="item.id"
        >
          <li>{{ item.id }}</li>
          <li>{{ item.name }}</li>
        </ul>
      </div>
    </div>
  </div>
  <!-- Card sorting province end -->
</template>
