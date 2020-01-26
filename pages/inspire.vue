<template>
  <section class="section">
    <nav class="panel">
      <div class="panel-heading is-inline-flex">
        <div>Repositories</div>
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
            Soemthing
            <h3 class="subtitle">Activities</h3>

            Soemthing
          </div>
        </div>
      </transition>
    </nav>
  </section>
</template>

<script>
export default {
  data() {
    return {
      dataBlock: true,
      isShow: false
    };
  },
  methods: {
    openCodeBlock() {
      this.dataBlock = !this.dataBlock;
    }
  }
};
</script>

