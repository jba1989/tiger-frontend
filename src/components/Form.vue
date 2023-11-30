<script setup lang="ts">
import { reactive } from "vue";

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

const emit = defineEmits(["submit"]);
const options = [
  { value: "KIX", label: "大阪" },
  { value: "NRT", label: "東京成田" },
  { value: "HND", label: "東京羽田" },
  { value: "IBR", label: "茨城" },
  { value: "OKJ", label: "岡山" },
  { value: "KCZ", label: "高知" },
  { value: "FUK", label: "福岡" },
  { value: "HSG", label: "佐賀" },
  { value: "OKA", label: "沖繩" },
  { value: "SDJ", label: "仙台" },
  { value: "AXT", label: "秋田" },
  { value: "HNA", label: "花卷" },
  { value: "HKD", label: "函館" },
  { value: "CTS", label: "新千歲" },
  { value: "AKJ", label: "旭川" },
  { value: "ICN", label: "首爾" },
  { value: "TAE", label: "大邱" },
  { value: "PUS", label: "釜山" },
  { value: "CJU", label: "濟州" },
];

function onSubmit() {
  emit("submit", form);
}
</script>

<template>
  <el-form :inline="false" :model="form" label-width="80px">
    <el-form-item label="Session ID">
      <el-input v-model="form.sessionId" />
    </el-form-item>
    <el-form-item label="目的地">
      <el-select
        v-model="form.destination"
        placeholder="起選擇目的地"
        multiple
        collapse-tags
      >
        <el-option
          v-for="item in options"
          :key="item.value"
          :label="item.label"
          :value="item.value"
        />
      </el-select>
    </el-form-item>
    <el-form-item label="出發日期">
      <el-date-picker
        v-model="form.since"
        type="date"
        placeholder="Pick a date"
        style="width: 100%"
        value-format="YYYY-MM-DD"
      />
    </el-form-item>
    <el-form-item label="回程日期">
      <el-date-picker
        v-model="form.until"
        type="date"
        placeholder="Pick a date"
        style="width: 100%"
        value-format="YYYY-MM-DD"
      />
    </el-form-item>
    <el-form-item style="float: right">
      <el-button type="primary" @click="onSubmit">搜尋</el-button>
    </el-form-item>
  </el-form>
</template>
