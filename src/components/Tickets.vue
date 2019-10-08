<template>
  <div class="c-wrap">
    <v-container>
      <v-row>
        <v-col cols="12" md="4">
          <div class="box" style="position: fixed;">
            <h4 class="title">Авиакомпании</h4>
            <v-checkbox
              class="items"
              @change="filter"
              v-for="(airline, index) in airlines"
              :key="index"
              v-model="selected"
              :label="airline.value"
              :value="airline.key"
            ></v-checkbox>
          </div>
        </v-col>
        <v-col cols="12" md="8" class="overflow">
          <div style="flex-direction:row;" v-for="(filt, i) in filtered" :key="i">
            <div class="box" style="margin-bottom:10px">
              <v-row class="rows">
                <div cols="12" md="6" class="info-ticket">{{filt.price}}</div>
                <div cols="12" md="8" class="info-accept">{{filt.itineraries[0][0].carrier_name}}</div>
              </v-row>
              <!-- <ul>
                <li v-for="(filt, i) in filtered" :key="i">{{filt.itineraries[0][0].carrier_name}}</li>
              </ul>-->
            </div>
          </div>
        </v-col>
      </v-row>
    </v-container>
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

<style scoped>
html,
body {
  background-color:#d7d7d7;
  padding: 0;
  margin: 0;
}

.c-wrap {
  background-color: #d7d7d7;
  height: 100%;
  width: 100%;
  position: relative;
  overflow: hidden;
  font-family: Open Sans;
  font-style: normal;
  font-weight: bold;
  font-size: 14px;
}

.title {
  margin-bottom: 20px;
}

.info-accept {
  display: flex;
  /* margin-left: 100px; */
  background: #d7d7d7;
  border-radius: 0px 4px 4px 0px;
}
.overflow {
  overflow: auto;
  height: 100%;
}
.info-ticket {
  display: flex;
  background: #f5f5f5;
  height: 100px;
  padding: 5px;
}

.box {
  background: #f5f5f5;
  padding: 15px;
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.15);
  border-radius: 4px;
}
.rows {
  width: 100%;
  height: 100%;
  justify-content: space-between;
}
.items {
  padding: 0;
  margin-top: -15px;
}
</style>