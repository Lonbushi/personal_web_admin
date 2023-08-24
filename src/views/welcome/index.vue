<script setup lang="ts">
import { getCaptcha } from "@/api/user";
import { reactive, computed, ref } from "vue";
import Bar from "./components/Bar.vue";
import Line from "./components/Line.vue";
import ViewBar from "./components/ViewBar.vue";
import Map from "./components/Map.vue";
import Pie from "./components/Pie.vue";
defineOptions({
  name: "Welcome"
});
getCaptcha();
const loading = ref<boolean>(true);
const UserData = ref(1000);
const ViewsData = ref(600);
const IPData = ref(300);
const ReaderData = ref(200);
const WebData = reactive([
  {
    id: 0,
    background: "linear-gradient(#536976, #BBD2C5)",
    icon: "icon-view",
    text: "浏览量",
    value: "ViewsData"
  },
  {
    id: 1,
    background: "linear-gradient(#005aa7, #aecfec)",
    icon: "icon-shuangren1",
    text: "访客量",
    value: "UserData"
  },
  {
    id: 2,
    background: "linear-gradient(#654EA3, #EAAFC8)",
    icon: "icon-ip",
    text: "IP数",
    value: "IPData"
  },
  {
    id: 3,
    background: "linear-gradient(#F2994A, #F2C94C)",
    icon: "icon-yuedu",
    text: "阅读量",
    value: "ReaderData"
  }
  // ... 其他数据项
]);
setTimeout(() => {
  loading.value = !loading.value;
}, 800);
const getValueForItem = valueKey => {
  switch (valueKey) {
    case "ViewsData":
      return ViewsData.value;
    case "UserData":
      return UserData.value;
    case "IPData":
      return IPData.value;
    case "ReaderData":
      return ReaderData.value;
    default:
      return "";
  }
};
</script>

<template>
  <div>
    <el-row :gutter="15">
      <el-col :xs="24" :sm="24" :md="24" :lg="14" :xl="14">
        <el-row :gutter="15">
          <el-col :xs="12" :sm="12" :md="12" :lg="6" :xl="6" v-for="item in WebData" :key="item.id">
            <el-card :style="{ background: item.background }">
              <span class="user">
                <svg class="icon" aria-hidden="true">
                  <use :xlink:href="'#' + item.icon"></use>
                </svg>
                <p>{{ item.text }}</p>
              </span>
              <div class="user_data">
                {{ getValueForItem(item.value) }}
              </div>
            </el-card>
          </el-col>
        </el-row>
        <el-col :xs="24" :sm="24" :md="24" :lg="24" :xl="24">
          <el-card style="margin-top: 20px">
            <template #header>
              <div class="card-header">
                <span>实时在线人数</span>
              </div>
            </template>
            <el-skeleton animated :rows="7" :loading="loading">
              <template #default>
                <Line />
              </template>
            </el-skeleton>
          </el-card>
        </el-col>
        <el-row> </el-row>
      </el-col>
      <el-col :xs="24" :sm="24" :md="24" :lg="10" :xl="10">
        <el-card style="height: 100%;">
          <template #header>
            <div class="card-header">
              <span>文章统计</span>
            </div>
          </template>
          <el-skeleton animated :rows="7" :loading="loading">
            <template #default>
              <Bar />
            </template>
          </el-skeleton>
        </el-card>
      </el-col>
    </el-row>
    <el-row style="margin-top: 20px" :gutter="15">
      <el-col :xs="24" :sm="24" :md="24" :lg="8" :xl="8"><el-card>
          <template #header>
            <div class="card-header">
              <span>访客分布排行榜</span>
            </div>
          </template>
          <el-skeleton animated :rows="7" :loading="loading">
            <template #default>
              <ViewBar />
            </template>
          </el-skeleton>
        </el-card>
      </el-col>
      <el-col :xs="24" :sm="24" :md="24" :lg="8" :xl="8">
        <el-card>
          <template #header>
            <div class="card-header">
              <span>访客地域分布</span>
            </div>
          </template>
          <el-skeleton animated :rows="7" :loading="loading">
            <template #default>
              <Map />
            </template>
          </el-skeleton>
        </el-card>
      </el-col>
      <el-col :xs="24" :sm="24" :md="24" :lg="8" :xl="8">
        <el-card>
          <template #header>
            <div class="card-header">
              <span>访问设备 </span>
            </div>
          </template>
          <el-skeleton animated :rows="7" :loading="loading">
            <template #default>
              <Pie />
            </template>
          </el-skeleton>
        </el-card>
      </el-col>
    </el-row>
  </div>
</template>
<style lang="scss" scoped>
.user {
  width: 100%;
  display: flex;
  justify-content: flex-start;
  color: white;
  align-items: center;

  p {
    font-size: 1.2rem;
  }
}

.card_icon {
  width: 2.5rem;
  height: 2.5rem;
  background-color: white;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-right: 20px;
}

.user_data {
  width: 100%;
  font-size: 1.4rem;
  color: white;
  text-align: center;
  margin-top: 6%;
}

.icon {
  width: 3em;
  height: 3em;
  margin-right: 5%;
}
</style>
