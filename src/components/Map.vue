<template>
  <div class="map" @click="this.unselectTable">
    <h3>Карта офиса</h3>
    <div v-if="!isLoading" class="map-root">
      <map-svg ref="map" />
      <table-svg v-show="false" ref="table" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import MapSvg from '../assets/images/map.svg';
import TableSvg from '../assets/images/workPlace.svg';
import { select as d3Select } from 'd3';
import tables from '../assets/data/tables.json';
import legend from '../assets/data/legend.json';

export default {
  name: 'Map',
  components: {
    MapSvg,
    TableSvg,
  },
  data() {
    return {
      isLoading: false,
      map: null,
      group: null,
      table: null,
      tables: [],
    };
  },
  created() {
    this.loadTables();
  },
  mounted() {
    this.isLoading = true;

    this.map = d3Select(this.$refs.map);
    this.group = this.map.select('g');
    this.table = d3Select(this.$refs.table);
    if (this.group) {
      this.drawTables();
    } else {
      alert('SVG is incorrect');
    }

    this.isLoading = false;
  },
  methods: {
    loadTables() {
      this.tables = tables;
    },
    drawTables() {
      const svgTablesGroupPlace = this.group
        .append('g')
        .classed('groupPlaces', true);

      this.tables.forEach((table) => {
        svgTablesGroupPlace
          .append('g')
          .attr(
            'transform',
            `translate(${table.x}, ${table.y}) scale(0.5) rotate(${
              table.rotate || 0
            })`
          )
          .attr('id', table._id)
          .attr('group_id', table.group_id)
          .classed('employer-place', true)
          .html(this.table.html())
          .attr(
            'fill',
            legend.find((legendItem) => legendItem.group_id === table.group_id)
              ?.color ?? 'transparent'
          )
          .on('click', this.selectTable);
      });
    },
    selectTable(e) {
      e.stopPropagation();
      this.$emit('selectTable', Number(e.currentTarget.id));
    },
    unselectTable() {
      this.$emit('unselectTable');
    },
  },
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
  padding: 24px;
  overflow: hidden;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

.map-root {
  height: 100%;
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

::v-deep svg {
  height: 100%;
  width: 100%;
}

::v-deep .table {
  cursor: pointer;
}
</style>
