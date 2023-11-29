<script lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';
import Chart from 'chart.js/auto';

export default {
  setup() {
    const chart = ref(null);
    const chartData = ref([
      {
        "origin": "FUK",
        "destination": "TPE",
        "date": "2023-11-08",
        "currency": "TWD",
        "amount": 1999,
        "fareLabels": [
            {
                "id": "13"
            }
        ]
    },
    {
        "origin": "FUK",
        "destination": "TPE",
        "date": "2023-11-09",
        "currency": "TWD",
        "amount": 1999,
        "fareLabels": [
            {
                "id": "13"
            }
        ]
    },
    {
        "origin": "FUK",
        "destination": "TPE",
        "date": "2023-11-10",
        "currency": "TWD",
        "amount": 2699,
        "fareLabels": []
    },
    {
        "origin": "FUK",
        "destination": "TPE",
        "date": "2023-11-11",
        "currency": "TWD",
        "amount": 4099,
        "fareLabels": []
    },
    {
        "origin": "FUK",
        "destination": "TPE",
        "date": "2023-11-12",
        "currency": "TWD",
        "amount": 4099,
        "fareLabels": []
    },
    {
        "origin": "FUK",
        "destination": "TPE",
        "date": "2023-11-13",
        "currency": "TWD",
        "amount": 3599,
        "fareLabels": []
    },
    {
        "origin": "FUK",
        "destination": "TPE",
        "date": "2023-11-14",
        "currency": "TWD",
        "amount": 1999,
        "fareLabels": [
            {
                "id": "13"
            }
        ]
    },
    ]);

    const createChart = () => {
      const ctx = chart.value?.getContext('2d');
      const dates = chartData.value.map(item => item.date);
      const amounts = chartData.value.map(item => item.amount);

      new Chart(ctx, {
        type: 'bar',
        data: {
          labels: dates,
          datasets: [
            {
              label: chartData.value[0]['origin'],
              data: amounts,
              backgroundColor: 'rgba(75, 192, 192, 0.2)',
              borderColor: 'rgba(75, 192, 192, 1)',
              borderWidth: 1,
            },
          ],
        },
      });
    };

    const fetchData = async () => {
      try {
        const url = 'https://iften6m0ub.execute-api.ap-southeast-2.amazonaws.com/default/tiger-api';
        const payload = {
          sessionId: "656763f75932483c7b03fc56",
          origin: "FUK",
          destination: "TPE",
          since: "2024-02-10",
          until: "2024-03-28",
        }
        const response = await axios.get(url, { params: payload });

        console.log(response.data)

        chartData.value = response.data?.data?.appLiveDailyPrices;
        createChart();
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    };

    onMounted(() => {
      fetchData();
      // createChart();
    });

    return {
      chart,
      chartData,
      fetchData,
      createChart,
    };
  },
};
</script>

<template>
  <div>
    <canvas ref="chart"></canvas>
  </div>
</template>

<style scoped>
/* Add your component styles here */
</style>