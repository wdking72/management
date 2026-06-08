<template>
  <div ref="chartRef" class="charts"></div>
</template>

<script setup lang="ts">
import * as echarts from 'echarts';
import { ref, computed, watch, onMounted, onUnmounted } from 'vue';
import type { PropType } from 'vue';

interface DataItem {
  id: string;
  project: string;
  overtime: boolean;
  hours: number;
  created_at: string;
}

type DataList = DataItem[];

const props = defineProps({
  data: {
    type: Array as PropType<DataList>,
    default: () => [],
  },
});

const chartRef = ref<HTMLElement | null>(null);
let chartInstance: echarts.ECharts | null = null;

// 计算项目名称和工时
const projects = computed(() => props.data.map((item) => item.project));
// 计算工时
const hours = computed(() => props.data.map((item) => item.hours));

const buildOption = () => ({
  title: {
    text: 'Project Hours Distribution',
  },
  tooltip: {},
  legend: {
    data: ['Hours'],
  },
  xAxis: {
    data: projects.value,
  },
  yAxis: {},
  series: [
    {
      name: 'Hours',
      type: 'bar',
      data: hours.value,
    },
  ],
});

const renderChart = () => {
  if (chartInstance) {
    chartInstance.setOption(buildOption(), true);
  }
};

const handleResize = () => {
  // 自适应图表大小
  chartInstance?.resize();
};

onMounted(() => {
  chartInstance = echarts.init(chartRef.value!);
  renderChart();
  // 监听窗口大小变
  window.addEventListener('resize', handleResize);
});

watch(
  // 监听 props.data 变化
  () => props.data,
  () => renderChart(),
  { deep: true }
);

onUnmounted(() => {
  // 组件卸载时，移除事件监听
  window.removeEventListener('resize', handleResize);
  chartInstance?.dispose();
  chartInstance = null;
});
</script>
