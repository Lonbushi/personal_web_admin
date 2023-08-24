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

const barChartRef = ref<HTMLDivElement | null>(null);
const { setOptions, resize } = useECharts(barChartRef as Ref<HTMLDivElement>, {
  theme
});
setOptions({
  tooltip: {
    trigger: "axis",
    axisPointer: {
      type: "shadow"
    }
  },
  xAxis: {},
  yAxis: {
    data: [
      "新乡市",
      "郑州市",
      "原阳市",
      "信阳市",
      "南阳市",
      "鹤壁市",
      "驻马店市"
    ]
  },
  series: [
    {
      type: "bar",
      data: [23, 24, 18, 25, 27, 28, 25]
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
  <div ref="barChartRef" style="width: 100%; height: 35vh"></div>
</template>
<style lang='scss' scoped>
/* 你可以根据需要添加或修改样式 */
</style>
