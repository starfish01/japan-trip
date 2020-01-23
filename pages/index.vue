<template>
  <span>
    <!-- <section class="section"> -->
    <!-- <div class="columns is-mobile"> -->
    <b-loading
      :is-full-page="isFullPage"
      :active.sync="isLoading"
      :can-cancel="true"
    ></b-loading>
    <template v-if="!isLoading">
      <div class="tabs is-centered">
        <ul>
          <li
            :class="{ 'is-active': index === selectedPage }"
            v-for="(data, index) in tripData"
            :key="data.city"
          >
            <a @click="selectedPage = index">{{ data.city }}</a>
          </li>
        </ul>
      </div>
      <section>
        <div class="columns is-mobile">
          <Location :tripData="tripData[selectedPage]" />
        </div>
      </section>
    </template>


  </span>
</template>

<script>
import Card from "~/components/Card";
import axios from "axios";
import Location from "@/components/Location";

export default {
  name: "HomePage",

  components: {
    Card,
    Location
  },
  data() {
    return {
      tripData: [],
      isLoading: false,
      isFullPage: true,
      selectedPage: 0
    };
  },
  methods: {},
  created() {
    this.isLoading = true;
    // call here
    axios
      .get(
        "https://v2-api.sheety.co/8901cb2a850b3d00684ba5b48efcb0a7/japanTrip/intinerary"
      )
      .then(({ data }) => {
        let workingData = data.intinerary;
        let outputData = [];

        _.forEach(workingData, (value, key) => {
          const exists = _.findIndex(outputData, o => {
            return o.city === value.city;
          });
          if (exists === -1) {
            outputData.push({
              city: value.city,
              transport: value.transport.split("\n"),
              startDate: value.date,
              endDate: value.apiEndDate,
              dates: [],
              accommodation: value.accommodation.split("\n")
            });
          }

          const restaurants = [value.restaurants, value.restaurants1];
          const activities = value.activity.split("\n");

          outputData[outputData.length - 1].dates.push({
            day: value.date,
            restaurants,
            activities
          });
        });
        this.isLoading = false;

        this.tripData = outputData;
      });
  }
};
</script>

<style lang="scss">
section {
  .columns {
    margin: 0 auto;
    width: 90vw;
  }
}
</style>
