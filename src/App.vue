<script setup lang="ts">
import { reactive, computed } from "vue";
import Form from "./components/Form.vue";
import TigerView from "./views/TigerView.vue";

interface FormType {
  sessionId: string;
  destination: Array<string>;
  since: string;
  until: string;
}

const form: FormType = reactive({
  sessionId: "",
  destination: [],
  since: "2024-03-30",
  until: "2024-05-30",
});

const active = computed(() => form.sessionId !== "" && form.destination.length > 0);

async function updateForm(newForm: FormType) {
  form.sessionId = newForm.sessionId;
  form.destination = newForm.destination;
  form.since = newForm.since;
  form.until = newForm.until;
}
</script>

<template>
  <header>
    <div class="wrapper">
      <Form @submit="updateForm" />
    </div>
  </header>

  <main>
    <TigerView v-if="active" :form="form" />
  </main>
</template>

<style scoped>
header {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 300px;
  padding: 10px;
}

main {
  position: relative;
  left: 300px;
  width: calc(100% - 300px);
  display: grid;
  grid-row-gap: 30px;
  grid-column-gap: 10px;
  grid-template-columns: repeat(2, 1fr);
}
</style>
