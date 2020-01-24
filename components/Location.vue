<template>
  <div class="location-container">
    <div class="location-header">
      <div class="location-title">
        <h1 class="title">{{ tripData.city }}</h1>
      </div>
      <div class="location-details">
        <h3>{{ tripData.startDate }} - {{ tripData.endDate }}</h3>
        <div class="accom">
          {{accomdetails}}
          <br />
          <a v-if="accomLink" :href="accomLink" target="_blank">{{accomTitle}}</a>
          <span v-if="!accomLink">{{ accomTitle }}</span> -
          <template v-if="accomAddress">
            <a :href="accomAddress" target="_blank">Map Location</a>
          </template>
        </div>
      </div>
    </div>

    <hr />
    <div v-for="item in tripData.dates" :key="item.day">
      <h3 class="title">{{ item.day }}</h3>
      <h3 class="subtitle">Restaurants</h3>
      <ul>
        <li v-for="res in item.restaurants" :key="res">{{ res }}</li>
      </ul>
      <br />
      <h3 class="subtitle">Activities</h3>
      <ul>
        <li v-for="res in item.activities" :key="res">{{ res }}</li>
      </ul>

      <hr />
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      accomdetails: "",
      accomLink: "",
      accomTitle: "",
      accomAddress: ""
    };
  },
  props: {
    tripData: {
      type: Object
    }
  },
  created() {
    this.accomDataFn();
  },
  methods: {
    accomDataFn() {
      _.forEach(this.tripData.accommodation, (value, key) => {
        if (value.includes("addressLink: ")) {
          this.accomAddress = value.replace("addressLink: ", "");
        } else if (value.includes("Link")) {
          this.accomLink = value.replace("Link: ", "");
        } else if (value.includes("Hotel")) {
          this.accomTitle = value.replace("Hotel: ", "");
        } else {
          this.accomdetails += value + " ";
        }
      });
    }
  }
};
</script>
<style lang="scss">
// @import '~bulma-Accordion';
.location-container {
  width: 100%;
}
.location-header {
  display: inline-flex;
  width: 100%;
  .location-title,
  .location-details {
    display: inline-block;
  }
  .location-title {
    width: 20%;
  }
  .location-details {
    width: 70%;
  }
}
</style>
