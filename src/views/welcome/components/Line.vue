<script setup lang="ts">
import { ref, computed, watch, type Ref } from "vue";
import { useAppStoreHook } from "@/store/modules/app";
import {
  delay,
  useDark,
  useECharts,
  type EchartOptions
} from "@pureadmin/utils";
// import * as echarts from "echarts/core";

const { isDark } = useDark();

const theme: EchartOptions["theme"] = computed(() => {
  return isDark.value ? "dark" : "light";
});

const lineChartRef = ref<HTMLDivElement | null>(null);
const { setOptions, resize } = useECharts(lineChartRef as Ref<HTMLDivElement>, {
  theme
});
setOptions({
  tooltip: {
    trigger: "axis",
    axisPointer: {
      type: "shadow"
    }
  },
  grid: {
    bottom: "20px",
    right: "10px"
  },
  // legend: {
  //   //@ts-expect-error
  //   right: true,
  //   data: ["watchers", "fork", "star"]
  // },
  xAxis: {
    type: 'time',
    splitLine: {
      show: false
    }
  },
  yAxis: {
    type: 'value',
    boundaryGap: [0, '100%'],
    splitLine: {
      show: false
    }
  },
  series: [
    {
      data: [0, 2, 80, 20, 10],
      type: "line",
      areaStyle: {
        color: "#ff0",
        opacity: 0.5
      }
    }
  ]
});
watch(
  () => useAppStoreHook().getSidebarStatus,
  () => {
    delay(600).then(() => resize());
  }
);
</script>
<template>
  <div ref="lineChartRef" style="width: 100%; height: 24vh" />
</template>
