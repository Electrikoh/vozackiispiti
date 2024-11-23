<template>
  <div class="wrapper">
    <div class="content">
      <h1 class="header">Статистики од испити (цела Македонија)</h1>

      <div class="chart-wrapper">
        <div class="chart-container" v-if="chartData" v-for="item in chartData">
          <a :href="getLinkForWebsite(item.type)" class="chart-link">
            <div class="chart-content">
              <h3 class="chart-title">{{ getTitle(item.type) }}</h3>
              <DonutChart
                class="chart"
                :labels="labels"
                :data="getChartData(item.stats)"
              />
              <h3 class="sample-size">
                бр. тестови : ({{
                  Object.values(item.stats).reduce((a, b) => a + b, 0)
                }})
              </h3>
            </div>
          </a>
        </div>
      </div>

      <p>Податоците се ажурирани: {{ chartData.latest_date }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useApiStore } from "~/store";

const store = useApiStore();
const chartData = ref([]);

onMounted(async () => {
  chartData.value = store.data.home;
  chartData.value.latest_date = store.data.latest_date;
});
function getChartData(stats) {
  return Object.values(stats);
}
const labels = ["на прва", "на втора", "на трета", "на 3+"];
const colors = ["#008ffb", "#00E396", "#FEB019", "#FF4560"];

function getLinkForWebsite(website) {
  if (website === "Теорија") return "/centri/teorija";
  if (website === "Град") return "/centri/grad";
  if (website === "Полигон") return "/centri/poligon";

  return "/error";
}
function getTitle(type) {
  if (type === "Теорија") return "Теоретски тест";
  if (type === "Град") return "Градско возење";
  if (type === "Полигон") return "Полигонско возење";

  return "/error";
}
</script>

<style scoped>
.info-cards {
  display: flex;
  gap: 1.5rem;
  justify-content: center;
  flex-wrap: wrap;
  margin: 2rem 0;
}

.card {
  background-color: white;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 1.5rem;
  text-align: center;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 300px;
  transition: transform 0.3s ease;
}

.card:hover {
  transform: scale(1.05);
}

.card h3 {
  font-size: 1.2rem;
  margin-bottom: 1rem;
  color: var(--text-color);
}

.card p {
  font-size: 1rem;
  color: var(--text-muted);
}

.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}

.content {
  background-color: white;
  width: 80%; /* Adjust the width for larger screens */
  max-width: 1200px; /* Set a maximum width for very large screens */
  margin: 0 auto; /* Center the content */
  border: 1px gray;
  box-shadow: 1px gray;
}

/* Add media queries for responsiveness */
@media screen and (max-width: 768px) {
  .content {
    width: 90%; /* Adjust the width for medium-sized screens */
  }
}

.chart-wrapper {
  font-family: "Lucida Sans", "Lucida Sans Regular", "Lucida Grande",
    "Lucida Sans Unicode", Geneva, Verdana, sans-serif;
  display: flex;
  justify-content: center; /* Center the chart containers horizontally */
  flex-wrap: wrap;
}
.chart-container {
  margin: 10px;
  border: 1px solid #ddd;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  text-align: center;
  max-width: 20rem;
  width: 100%;
  cursor: pointer;
  transition: all 0.3s ease;
}
.chart-container:hover {
  transform: scale(1.05); /* Increase size on hover */
}
.chart-title {
  font-size: 1.5rem;
  margin-bottom: 10px;
}

.chart {
  max-width: 300px;
  margin: 0 auto;
}

.sample-size {
  margin-top: 10px;
  font-size: 1rem;
  color: #888;
}

.subtitle {
  font-size: 1.2rem;
  color: #3d3d3d;
  margin: 1.6rem 4rem;
  text-align: center;
}

.header {
  margin-top: 2rem;
  text-align: center;
  color: black;
}

#latest {
  text-align: right;
  margin-right: 4rem;
  font-size: 1rem;
  color: gray;
}
.chart-link {
  display: block;
  color: inherit;
  text-decoration: none;
}

@media only screen and (max-width: 600px) {
  .subtitle {
    display: none;
  }
}
</style>
