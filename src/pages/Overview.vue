<script setup>
import { ref } from "vue";
import provinces from "../assets/js/province.js";
import dataCity from "../assets/js/city";

const provinceId = ref(0);
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
  <div class="overview- flex">
    <div class="section border">
      <h2 class="text-xl font-medium leading-[19.36px] text-[#424242]">
        Show Cities by Province ID
      </h2>
      <label for="provinceId">Province ID:</label>
      <input v-model="provinceId" type="number" />
      <button @click="showCities">Show Cities</button>
      <ul v-for="item in cities" :key="item.id">
        <li>{{ item.name }}</li>
        <li>{{ item.id }}</li>
      </ul>
    </div>
    <div class="section border">
      <h2>Show Province by City ID:</h2>
      <label for="cityId">City ID:</label>
      <input v-model="cityId" type="number" />
      <button @click="showProvince">Show Province</button>
      <div class="result">
        <p>Province Name:</p>
        <p>
          {{ provinceName }}
        </p>
      </div>
      {
    </div>
    <div class="section">
      <h2>Sort Provinces:</h2>
      <label for="sortType">Sort by:</label>
      <select v-model="sortType">
        <option value="id_asc">ID (Ascending)</option>
        <option value="id_desc">ID (Descending)</option>
        <option value="name_asc">Name (Ascending)</option>
        <option value="name_desc">Name (Descending)</option>
      </select>
      <button @click="sortProvinces">Sort</button>
      <table>
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
      </table>
    </div>
  </div>
</template>

<style scoped>
/* CSS untuk desain UI */
body {
  font-family: Arial, sans-serif;
}
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}
h2 {
  margin-top: 0;
}
label {
  font-weight: bold;
}
input[type="text"] {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
}
button {
  background-color: #4caf50;
  color: white;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #45a049;
}
.result {
  margin-top: 20px;
}
</style>
