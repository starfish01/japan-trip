<template>
  <div>
    <h1 class="title">{{ tripData.city }}</h1>
    <h3>{{ tripData.startDate }} - {{ tripData.endDate }}</h3>
    <div class="accom">
      <ul>
        <li v-for="(accomData, index) in tripData.accommodation" :key="index">
          {{ accomData }}
        </li>
      </ul>
    </div>
    <hr />
    <div v-for="item in tripData.dates" :key="item.day">
      <h3 class="title">{{ item.day }}</h3>
      <h3 class="subtitle">Restaurants</h3>
      <ul>
        <li v-for="res in item.restaurants" :key="res">
          {{ res }}
        </li>
      </ul>
      <br />
      <h3 class="subtitle">Activities</h3>
      <ul>
        <li v-for="res in item.activities" :key="res">
          {{ res }}
        </li>
      </ul>

      <hr />
    </div>
  </div>
</template>
<script>
export default {
  props: {
    tripData: {
      type: Object
    }
  }
};
</script>
<style lang="scss">
// @import '~bulma-Accordion';
</style>
