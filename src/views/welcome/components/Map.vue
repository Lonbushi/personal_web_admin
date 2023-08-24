<script setup lang='ts'>
import { ref, computed, watch, type Ref, onMounted, onBeforeUnmount } from "vue";
// import { useAppStoreHook } from "@/store/modules/app";
import { delay, useDark, useECharts } from "@pureadmin/utils";
import * as echarts from "echarts/core";
import mapData from "@/assets/map/ChinaMap";
import { MapChart } from 'echarts/charts';

echarts.use([MapChart]);

const { isDark } = useDark();

const theme = computed(() => {
  return isDark.value ? "dark" : "light";
});

const mapChartRef = ref<HTMLDivElement | null>(null);
const { setOptions, resize } = useECharts(mapChartRef as Ref<HTMLDivElement>, {
  theme
});




const ChinaStyleColors = [
  '#50a3ba', '#eac763', '#d94e5d', '#f6bb42', '#57a3e8', '#7266ba'
];

const mapOptions = {
  series: [
    {
      name: 'ChinaMap',
      type: 'map',
      map: 'chinaMap',
      roam: true,
      zoom: 1.25,
      data: [
        { name: '河南省', value: 400 },
        { name: '山西省', value: 220 }
      ],
    }
  ],
  //普通样式
  itemStyle: {
  },
  tooltip: {
    // show: true,
    trigger: 'item',
    showDelay: 0,
    transitionDuration: 0.2
  },
  visualMap: {
    min: 0,
    max: 500,
    calculable: true,
    color: ChinaStyleColors,
    textStyle: {
      color: '#c97586'
    },
    text: ['高', '低'],
    inRange: {
      color: ['#e4d2d8', '#c97586']
    },
  },
  toolbox: {
    show: true,
  },
  emphasis: {
    label: {
      show: true
    }
  },

};

onMounted(() => {
  echarts.registerMap('chinaMap', mapData);
  setOptions(mapOptions);
  // 页面大小变化时重绘图表
  const handleResize = () => {
    resize(); // 你已经从 useECharts 解构出了 resize 函数
  };

  window.addEventListener('resize', handleResize);

  // 当组件销毁时移除事件监听，以避免内存泄漏
  onBeforeUnmount(() => {
    window.removeEventListener('resize', handleResize);
  });
});
</script>

<template>
  <div id="mapChart" ref="mapChartRef" style="width: 100%; height: 35vh"></div>
</template>

<style lang='scss' scoped>
/* 你可以根据需要添加或修改样式 */
</style>

