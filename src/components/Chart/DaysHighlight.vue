<!--
/**
 * @fileoverview Days highlight component
 * @license MIT
 * @author Rafal Pospiech <neuronet.io@gmail.com>
 * @package GanttElastic
 */
-->

<template>
  <g
    class="gantt-elastic__chart-days-highlight-container"
    :style="{ ...root.style['chart-days-highlight-container'] }"
    v-if="showWorkingDays"
  >
    <template v-for="day in workingDays">
      <rect
        :key="getKey(day)+'_1'"
        class="gantt-elastic__chart-days-highlight-rect"
        :x="day.offset.px"
        y="0"
        :width="day.width.px"
        height="100%"
        :style="{ ...root.style['chart-days-highlight-rect']}"
      ></rect>
      <rect
        v-if="dayjs(day.time).day() == 0"
        :key="getKey(day)+'_2'"
        _class="gantt-elastic__chart-days-highlight-rect"
        :x="day.offset.px + day.width.px"
        y="0"
        :width="1"
        height="100%"
        :style="{ fill: '#666', 'z-index': 200}"
      ></rect>
    </template>
  </g>
</template>

<script>
import dayjs from 'dayjs';
export default {
  name: 'DaysHighlight',
  inject: ['root'],
  data() {
    return {
      dayjs
    };
  },
  methods: {
    /**
     * Get key
     *
     * @param {object} day
     * @returns {string} key ideintifier for loop
     */
    getKey(day) {
      return dayjs(day.time).format('YYYY-MM-DD');
    }
  },
  computed: {
    /**
     * Get working days
     *
     * @returns {array}
     */
    workingDays() {
      return this.root.state.options.times.steps.filter(step => {
        return this.root.state.options.calendar.workingDays.indexOf(dayjs(step.time).day()) === -1;
      });
    },

    /**
     * Show working days?
     *
     * @returns {bool}
     */
    showWorkingDays() {
      const calendar = this.root.state.options.calendar;
      if (
        typeof calendar.workingDays !== 'undefined' &&
        Array.isArray(calendar.workingDays) &&
        calendar.workingDays.length
      ) {
        return true;
      }
      return false;
    }
  }
};
</script>
