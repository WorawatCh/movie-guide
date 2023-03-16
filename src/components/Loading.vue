<script setup>
import ProgressSpinner from 'primevue/progressspinner';
const props = defineProps({
  isLoading: Boolean,
});
</script>

<template>
<div class="loading" v-if="isLoading">
    <ProgressSpinner aria-label="Loading" style="top: 50vh; left: 50%;" strokeWidth="8" animationDuration=".5s" />
  </div>
</template>

<style scoped>
.loading{
    position: relative;
    z-index: 30000;
    width: 100vw;
    height: 100vh;
    opacity: 0.5;
    background: #E5E4E2;
}
</style>