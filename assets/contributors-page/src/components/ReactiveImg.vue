<template>
  <div class="wrapper">
    <v-progress-circular indeterminate color="primary" v-show="!loaded"></v-progress-circular>
    <transition name="fade">
      <img class="icon" :src="src" :title="title" @load="onLoaded" v-show="loaded"  width="100%" height="100%">
    </transition>
  </div>

</template>

<script>
export default {
  props: ['src', 'title'],
  data() {
    return {
      loaded: false
    };
  },
  methods: {
    onLoaded() {
      this.loaded = true;
    }
  }
};
</script>

<style>
.fade-enter-active {
  transition: opacity 1s ease-in-out;
}

.fade-enter-to {
  opacity: 1;
}

.fade-enter {
  opacity: 0;
}
</style>
