<script setup lang="ts">
import { ref, onMounted } from "vue";
import axios from "axios";
import Chart from "chart.js/auto";
import { ElNotification } from "element-plus";
import { Refresh } from "@element-plus/icons-vue";

const prop = defineProps({
  sessionId: {
    type: String,
    required: true,
  },
  origin: {
    type: String,
    required: true,
  },
  destination: {
    type: String,
    required: true,console
  },
  since: {
    type: String,
    required: true,
  },
  until: {
    type: String,
    required: true,
  },
});
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
  { value: "ICN", label: "首爾(仁川)" },
  { value: "TAE", label: "大邱" },
  { value: "PUS", label: "釜山" },
  { value: "CJU", label: "濟州" },
  { value: "TPE", label: "台北(桃園)" },
];

function findLabel(value: string) {
  return options.find((item) => item.value === value)?.label;
}

const chart = ref(null);
const chartData = ref([
  {
    origin: "FUK",
    destination: "TPE",
    date: "2023-11-08",
    currency: "TWD",
    amount: 1999,
    fareLabels: [
      {
        id: "13",
      },
    ],
  },
  {
    origin: "FUK",
    destination: "TPE",
    date: "2023-11-09",
    currency: "TWD",
    amount: 1999,
    fareLabels: [
      {
        id: "13",
      },
    ],
  },
  {
    origin: "FUK",
    destination: "TPE",
    date: "2023-11-10",
    currency: "TWD",
    amount: 2699,
    fareLabels: [],
  },
  {
    origin: "FUK",
    destination: "TPE",
    date: "2023-11-11",
    currency: "TWD",
    amount: 4099,
    fareLabels: [],
  },
  {
    origin: "FUK",
    destination: "TPE",
    date: "2023-11-12",
    currency: "TWD",
    amount: 4099,
    fareLabels: [],
  },
  {
    origin: "FUK",
    destination: "TPE",
    date: "2023-11-13",
    currency: "TWD",
    amount: 3599,
    fareLabels: [],
  },
  {
    origin: "FUK",
    destination: "TPE",
    date: "2023-11-14",
    currency: "TWD",
    amount: 1999,
    fareLabels: [
      {
        id: "13",
      },
    ],
  },
]);

const createChart = () => {
  const dates = chartData.value.map((item) => item.date.split("-").slice(1).join("/"));
  const amounts = chartData.value.map((item) => item.amount);

  if (chart.value) {
    chart.value.destroy();
  }

  chart.value = new Chart(prop.origin + prop.destination, {
    type: "bar",
    data: {
      labels: dates,
      datasets: [
        {
          label: `${findLabel(prop.origin)} -> ${findLabel(prop.destination)}`,
          data: amounts,
          backgroundColor: "rgba(75, 192, 192, 0.2)",
          borderColor: "rgba(75, 192, 192, 1)",
          borderWidth: 1,
        },
      ],
    },
    options: {
      maintainAspectRatio: true, // 禁用維持長寬比
      responsive: true, // 啟用響應式調整大小
    },
  });
};

const fetchData = async () => {
  try {
    const url =
      "https://iften6m0ub.execute-api.ap-southeast-2.amazonaws.com/default/tiger-api";
    const payload = {
      sessionId: prop.sessionId,
      origin: prop.origin,
      destination: prop.destination,
      since: prop.since,
      until: prop.until,
    };
    const response = await axios.get(url, { params: payload });

    if (!response.data?.data?.appLiveDailyPrices?.length) {
      ElNotification({
        title: "Warning",
        message: "Session ID 已過期，請重新輸入",
        type: "warning",
      });

      throw new Error("Session ID 已過期，請重新輸入");
    }

    chartData.value = response.data?.data?.appLiveDailyPrices;
    createChart();
  } catch (error) {
    console.error("Error fetching data:", error);
  }
};

onMounted(() => {
  fetchData();
  // createChart();
});
</script>

<template>
  <div>
    <el-button
      :icon="Refresh"
      size="small"
      type="primary"
      plain
      circle
      @click="fetchData"
    />
    <canvas :id="prop.origin + prop.destination"></canvas>
  </div>
</template>

<style scoped>
canvas {
  height: 100%;
}
</style>
