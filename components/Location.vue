<template>
  <div class="location-container">
    <div class="location-header">
      <div class="location-title">
        <h1 class="title">{{ tripData.city }}</h1>
      </div>
      <div class="location-details">
        <h3>{{ tripData.startDate }} - {{ tripData.endDate }}</h3>
        <div class="accom">
          {{ accomdetails }}
          <br />
          <a v-if="accomLink" :href="accomLink" target="_blank">{{
            accomTitle
          }}</a>
          <span v-if="!accomLink">{{ accomTitle }}</span> -
          <template v-if="accomAddress">
            <a :href="accomAddress" target="_blank">Map Location</a>
          </template>
        </div>
      </div>
    </div>

    <hr />

    <nav class="panel">
      <div class="panel-heading is-inline-flex">
        <div>Restaurants</div>
        <a
          :class="{ 'button-active': isShow }"
          @click="isShow = !isShow"
          class="panel-action-button"
          ><b-icon icon="arrow-down" size="is-small" type="is-primary"
        /></a>
      </div>

      <transition name="slide">
        <div v-if="isShow" class="panel-block child">
          <div style="padding:10px">
            <ul>
              <li v-for="res in restaurantList" :key="res">{{ res }}</li>
            </ul>
          </div>
        </div>
      </transition>
    </nav>
    <nav class="panel">
      <div class="panel-heading is-inline-flex">
        <div>Activites</div>
        <a
          :class="{ 'button-active': isShowActivities }"
          @click="isShowActivities = !isShowActivities"
          class="panel-action-button"
          ><b-icon icon="arrow-down" size="is-small" type="is-primary"
        /></a>
      </div>

      <transition name="slide">
        <div v-if="isShowActivities" class="panel-block child">
          <div style="padding:10px">
            <ul>
              <li v-for="res in activityList" :key="res">{{ res }}</li>
            </ul>
          </div>
        </div>
      </transition>
    </nav>

    <hr />

    <nav class="panel" v-for="item in tripData.dates" :key="item.day">
      <div class="panel-heading is-inline-flex">
        <div>{{ item.day }}</div>
        <a
          v-if="item.activities.length && item.activities[0].length"
          :class="{ 'button-active': item.display }"
          @click="item.display = !item.display"
          class="panel-action-button"
          ><b-icon icon="arrow-down" size="is-small" type="is-primary"
        /></a>
        <span v-else class="panel-action-button">No Plans</span>
      </div>

      <transition name="slide">
        <div v-if="item.display" class="panel-block child">
          <div style="padding:10px">
            <ul>
              <li v-for="res in item.activities" :key="res">{{ res }}</li>
            </ul>
          </div>
        </div>
      </transition>
    </nav>
  </div>
</template>
<script>
export default {
  data() {
    return {
      accomdetails: "",
      accomLink: "",
      accomTitle: "",
      accomAddress: "",
      restaurantList: [],
      activityList: [],
      isShowActivities: false,
      isShow: false
    };
  },
  props: {
    tripData: {
      type: Object
    }
  },
  mounted() {
    this.accomDataFn();
    this.getRestaurants();
    this.getActivities();
  },
  methods: {
    accomDataFn() {
      console.log(this.tripData.accommodation);
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
    },
    getRestaurants() {
      let formatedData = [];

      _.forEach(this.tripData.dates, (value, key) => {
        formatedData.push(value.restaurants);
      });

      this.restaurantList = _.spread(_.union)(formatedData);
    },
    getActivities() {
      let formatedData = [];

      _.forEach(this.tripData.dates, (value, key) => {
        formatedData.push(value.activities);
      });

      this.activityList = _.spread(_.union)(formatedData);
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
    white-space: nowrap;
  }
  .location-details {
    padding-left: 10px;
  }
}
</style>
