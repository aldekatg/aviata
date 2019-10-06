<template>
  <div>
    <v-layout row class="justify-space-between">
      <v-flex xs3>
        <div class="filters" id="box">
          <h4>Авиакомпании</h4>
          <div>
            <v-checkbox
              @change="filter"
              v-for="(airline, index) in airlines"
              :key="index"
              class="checkbox-input"
              v-model="selected"
              color="success"
              :label="airline.value"
              :value="airline.key"
            ></v-checkbox>
          </div>
        </div>
      </v-flex>
      <v-flex xs1></v-flex>
      <v-flex xs6>
        <ul>
          <li v-for="(filt, i) in filtered" :key="i">{{filt.price}}</li>
        </ul>
      </v-flex>
    </v-layout>
  </div>
</template>;

<script>
const dataJson = require("../assets/results.json");
export default {
  selected: [],
  data() {
    return {
      buttons: [],
      airlines: [],
      selected: [],
      flights: dataJson.flights,
      filtered: []
    };
  },
  mounted: function() {
    console.log(dataJson);
    this.getAirlines();
  },
  watch: {
    filtered: function() {
      console.log(this.filtered.length);
    }
  },
  methods: {
    getAirlines() {
      for (let data in dataJson.airlines) {
        this.airlines.push({ key: data, value: dataJson.airlines[data] });
      }
    },

    filter() {
      this.filtered = [];
      if (this.selected.length > 0) {
        for (let select of this.selected) {
          this.flights.filter(item => {
            if (item.validating_carrier == select) {
              this.filtered.push(item);
              return item;
            }
          });
        }
      }
    }
  }
};
</script>

<style>
html,
body {
  padding: 0;
  margin: 0;
  background: #d7d7d7;
  font-family: Open Sans;
  font-style: normal;
  font-weight: bold;
  font-size: 14px;
}

* {
  box-sizing: border-box;
}
.v-input__slot .v-label {
  color: black !important;
}
.checkbox-input {
  height: 50px;
  margin: 0 !important;
  padding: 0 8px !important;
}
div .v-input {
  height: 25px !important;
}
div > .v-messages {
  display: none;
}
#wrapper {
  width: 100%;
  max-width: 1100px;
  margin: 0 auto;
}

.filters {
  float: left;
  padding: 20px;
}

#tickets {
  width: 63.63636363636364%;
  float: left;
  padding: 20px;
  text-align: start;
}

#box {
  background: #f5f5f5;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.15);
  border-radius: 4px;
}

@media screen and (max-width: 500px) {
  #filters,
  #tickets {
    float: none;
    width: 100%;
  }
}
</style>