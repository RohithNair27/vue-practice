<script>
import "./style.css";
import { Api } from "./components/PlanesAPI";
export default {
  methods: {
    onCall() {
      this.orginalData = Api;
      this.PlanesData = Api;
      this.inputText = "";
      this.selected = "";
      this.capacitySelected = "";
    },

    getData() {
      this.orginalData = Api;
      this.PlanesData = Api;
    },

    // fuction to filter both based on text and drop down
    onFilter() {
      this.PlanesData = this.orginalData.filter((element) => {
        let textSearched = element.model
          .toLowerCase()
          .includes(this.inputText.toLocaleLowerCase());
        let manufacturer = true;
        let capacity = true;
        if (this.selected !== "") {
          manufacturer = element.manufacturer === this.selected;
        }
        if (this.capacitySelected !== "") {
          if (this.capacitySelected == "100") {
            capacity = element.capacity <= 100;
          } else if (this.capacitySelected == "500") {
            capacity = element.capacity > 100 && element.capacity <= 500;
          } else {
            capacity = element.capacity > "500";
          }
        }
        return textSearched && manufacturer && capacity;
      });
    },
  },
  data: function states() {
    return {
      orginalData: [],
      PlanesData: [],
      inputText: "",
      selected: "",
      capacitySelected: "",
      isLoading: "",
    };
  },
  created: function onCreate() {
    this.getData();
  },
};
</script>

<template>
  <div id="body">
    <nav class="navbar bg-body-tertiary">
      <div class="container">
        <a class="navbar-brand" href="#">
          <img
            src="./assets/plane-svgrepo-com.svg"
            alt="Bootstrap"
            width="80"
            height="80"
          />
          <h1>Plane tracker</h1>
        </a>
      </div>
    </nav>
    <section class="input-container">
      <input
        v-model="inputText"
        placeholder="search for model"
        type="text"
        @input="(event) => (inputText = event.target.value)"
      />
      <select name="manufacturer" class="dropdown" v-model="selected">
        <option disabled value="">Please select manufacturer</option>
        <option v-for="plane in orginalData" :value="plane.manufacturer">
          {{ plane.manufacturer }}
        </option>
      </select>
      <select name="capacity" class="dropdown" v-model="capacitySelected">
        <option disabled value="">Please select capacity</option>
        <option value="100">less than 100</option>
        <option value="500">between 100 and 500</option>
        <option value="1000">More 500</option>
      </select>
      <button type="button" class="btn btn-primary btn-lg" @click="onFilter">
        Filter
      </button>
      <button @click="onCall" class="btn btn-danger" type="button">
        Reset
      </button>
    </section>
    <section class="planes-container">
      <table id="planes-table">
        <tr>
          <th>model</th>
          <th>manufacturer</th>
          <th>year manufatured</th>
          <th>capacity</th>
          <th>range_km</th>
        </tr>
        <tr v-for="plane in PlanesData">
          <td>{{ plane.model }}</td>
          <td>{{ plane.manufacturer }}</td>
          <td>{{ plane.year }}</td>
          <td>{{ plane.capacity }}</td>
          <td>{{ plane.range_km }}</td>
        </tr>
      </table>
    </section>
  </div>
</template>
