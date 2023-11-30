<script setup lang="ts">
import Tiger from "../components/Tiger.vue";

const prop = defineProps({
  form: {
    type: Object,
    required: true,
  },
});
</script>

<template>
  <div v-for="des in form.destination" style="padding: 3px; border: 1px dashed #cecece">
    <div class="canvas-container">
      <Tiger
        :sessionId="form.sessionId"
        origin="TPE"
        :destination="des"
        :since="form.since"
        :until="form.until"
      />
    </div>
    <div class="canvas-container">
      <Tiger
        :sessionId="form.sessionId"
        :origin="des"
        destination="TPE"
        :since="form.since"
        :until="form.until"
      />
    </div>
  </div>
</template>

<style scoped>
.cansvas-container {
  width: auto;
  height: 340px;
}
</style>
