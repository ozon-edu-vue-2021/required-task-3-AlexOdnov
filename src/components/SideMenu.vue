<template>
  <div class="menu">
    <div class="toolbar">
      <div class="toolbar__header">
        <template v-if="!isUserOpened">
          <h3>Информация</h3>
        </template>
        <template v-else>
          <div class="action" @click="closeProfile">
            <div class="arrow"></div>
          </div>
          <h3>Профиль</h3>
        </template>
      </div>
    </div>
    <div class="content">
      <div v-show="!isUserOpened" class="legend">
        <div class="legend__data">
          <Draggable
            v-if="legend.length > 0"
            v-model="legend"
            class="legend__items"
          >
            <legend-item
              v-for="(item, index) in legend"
              :key="index"
              :color="item.color"
              :text="item.text"
              :counter="item.counter"
              class="legend__item"
            />
          </Draggable>
          <span v-else class="legend--empty">Список пуст</span>
        </div>
        <div class="legend__chart">
          <pie-chart ref="chart" />
        </div>
      </div>
      <div v-show="isUserOpened" class="profile">
        <div v-if="!person" class="profile__empty">Место пустое</div>
        <person-card v-else :person="person" />
      </div>
    </div>
  </div>
</template>

<script>
import LegendItem from './SideMenu/LegendItem.vue';
import PersonCard from './SideMenu/PersonCard.vue';
import legend from '../assets/data/legend.json';
import Draggable from 'vuedraggable';
import { Doughnut as PieChart } from 'vue-chartjs';

export default {
  name: 'SideMenu',
  props: {
    isUserOpened: {
      type: Boolean,
      default: false,
    },
    person: {
      type: Object,
      default: null,
    },
  },
  components: {
    LegendItem,
    PersonCard,
    Draggable,
    PieChart,
  },
  data() {
    return {
      legend: [],
    };
  },
  created() {
    this.loadLegend();
  },
  mounted() {
    if (this.$refs.chart) {
      this.makeChart();
    }
  },
  methods: {
    loadLegend() {
      this.legend = legend;
    },
    closeProfile() {
      this.$emit('update:isUserOpened', false);
    },
    makeChart() {
      const legendChartData = {
        labels: this.legend.map((legendItem) => legendItem.text),
        datasets: [
          {
            label: 'Легенда',
            backgroundColor: this.legend.map((legendItem) => legendItem.color),
            data: this.legend.map((legendItem) => legendItem.counter),
          },
        ],
      };

      const options = {
        borderWidth: '10px',
        legend: {
          display: false,
        },
      };

      this.$refs.chart.renderChart(legendChartData, options);
    },
  },
};
</script>

<style scoped>
.menu {
  border-left: 1px solid #ccd8e4;
  padding: 24px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.toolbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.toolbar__header {
  display: flex;
  align-items: center;
  margin-bottom: 12px;
}

.toolbar__header .action {
  cursor: pointer;
  margin-right: 14px;
  width: 20px;
  height: 20px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.toolbar__header .action .arrow {
  width: 10px;
  height: 10px;
  border-top: 2px solid blue;
  border-right: 2px solid blue;
  transform: rotate(-135deg);
}

h3 {
  margin: 0;
}

.content {
  flex: 1;
  padding-right: 10px;
  overflow: auto;
  --scrollbarBG: transparent;
  --thumbBG: #255af6;
  scrollbar-width: thin;
  scrollbar-color: var(--thumbBG) var(--scrollbarBG);
}
.content::-webkit-scrollbar {
  width: 6px;
}
.content::-webkit-scrollbar-track {
  background: var(--scrollbarBG);
}
.content::-webkit-scrollbar-thumb {
  background-color: var(--thumbBG);
  border-radius: 6px;
}

.content .legend {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 100%;
  gap: 1rem;
}

.content .legend .legend__data {
  display: flex;
  height: 100%;
}

.content .legend .legend__items {
  flex: 1;
  width: 100%;
}

.content .legend .legend__items .legend__item:not(:first-child) {
  margin-top: 16px;
}

.content .legend .legend__items .legend__item {
  cursor: pointer;
}

.content .legend .legend__items .legend__item.sortable-chosen {
  opacity: 25%;
}

.content .legend .legend--empty {
  align-self: center;
  width: 100%;
  text-align: center;
}
</style>
