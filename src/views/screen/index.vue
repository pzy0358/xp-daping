<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import * as echarts from 'echarts'

import { option } from '../../utils/map'
import { onMounted, ref } from 'vue'
// import '../../utils/lib/china'
import 'echarts/map/js/china.js'
import { Vue3SeamlessScroll } from 'vue3-seamless-scroll'

const list = ref([
  { title: '无缝滚动第一行无缝滚动第一行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第二行无缝滚动第二行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第三行无缝滚动第三行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第四行无缝滚动第四行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第五行无缝滚动第五行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第六行无缝滚动第六行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第七行无缝滚动第七行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第八行无缝滚动第八行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第九行无缝滚动第九行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第九行无缝滚动第九行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第一行无缝滚动第一行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第二行无缝滚动第二行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第三行无缝滚动第三行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第四行无缝滚动第四行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第五行无缝滚动第五行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第六行无缝滚动第六行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第七行无缝滚动第七行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第八行无缝滚动第八行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第九行无缝滚动第九行', date: '2023-5-10 18:09:22' },
  { title: '无缝滚动第九行无缝滚动第九行', date: '2023-5-10 18:09:22' }
])

let chart = ref()
const index = ref(0)

const handleTab = (val) => {
  index.value = val
}

const initChart = () => {
  chart.value = echarts.init(document.getElementById('geo-chart'))
  chart.value.setOption(option)
}

const onResize = () => {
  chart.value.resize()
}

window.addEventListener('resize', onResize)

onMounted(() => {
  initChart()
})

const data = ref([
  {
    name: '365天',
    day: { orders: '20,301,987', amount: '99834' }
  },
  {
    name: '90天',
    day: { orders: '301,987', amount: '9834' }
  },
  {
    name: '30天',
    day: { orders: '1,987', amount: '3834' }
  },
  {
    name: '24小时',
    day: { orders: '987', amount: '834' }
  }
])

const activeIndex = ref(0)

const handleActive = (item, val) => {
  activeIndex.value = val
  getChildren(item)
}

const children = ref({})

const getChildren = (item) => {
  console.log('name', item['day'])
  children.value = item['day']
}
getChildren(data.value[activeIndex.value])
</script>
<template>
  <div class="viewport">
    <div class="column">
      <div class="overview panel">
        <div class="inner">123</div>
      </div>
      <div class="monitor panel">
        <div class="inner flex flex-column">
          <div class="tabs">
            <span @click="handleTab(0)" :class="{ active: index === 0 }">实时交易详情</span>
            <span class="line">|</span>
            <span @click="handleTab(1)" :class="{ active: index === 1 }">实时注册详情</span>
          </div>
          <div class="container" v-if="index === 0">
            <Vue3SeamlessScroll class="v-s-s" :list="list" :hover="true" :step="0.5">
              <div class="v-s-s_item" v-for="(item, index) in list" :key="index">
                <span>{{ item.title }}</span>
                <span>{{ item.date }}</span>
              </div>
            </Vue3SeamlessScroll>
          </div>
          <div class="container" v-if="index === 1">
            <Vue3SeamlessScroll class="v-s-s" :list="list" :hover="true" :step="0.5">
              <div class="v-s-s_item" v-for="(item, index) in list" :key="index">
                <span>{{ item.title }}</span>
                <span>{{ item.date }}</span>
              </div>
            </Vue3SeamlessScroll>
          </div>
        </div>
      </div>
      <div class="point panel">
        <div class="inner">销售地统计</div>
      </div>
    </div>
    <div class="column">
      <div class="map">
        <h3 class="title">设备数据统计</h3>
        <div class="geo" id="geo-chart"></div>
      </div>
      <div class="users panel">
        <div class="inner">全国用户总量统计</div>
      </div>
    </div>
    <div class="column">
      <div class="order panel">
        <div class="inner">
          <div class="tabs">
            <template v-for="(item, index) in data" :key="index">
              <span @click="handleActive(item, index)" :class="{ active: activeIndex === index }">{{
                item.name
              }}</span>
              <span v-if="index !== data.length - 1" class="line">|</span>
            </template>
          </div>
          <div class="content">
            {{ children.orders }}
            <br />
            {{ children.amount }}
          </div>
        </div>
      </div>
      <div class="sales panel">
        <div class="inner">2</div>
      </div>
      <div class="wrap">
        <div class="channel panel">
          <div class="inner">渠道</div>
        </div>
        <div class="quarter panel">
          <div class="inner">销售进度</div>
        </div>
      </div>
      <div class="top panel">
        <div class="inner">全国热榜</div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.v-s-s {
  height: 100%;
  width: 100%;

  overflow: hidden;
  font-size: 13px;
  // background-color: salmon;

  .v-s-s_item {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 3px 0;
  }
}
.overview {
  height: 0.5729rem;
  // margin-bottom: 0.1042rem;
}

.monitor {
  height: 2.5rem;
  // margin-bottom: 0.1042rem;
}

.point {
  height: 1.7708rem;
}

.map {
  height: 3.0469rem;
  margin-bottom: 0.1042rem;
  display: flex;
  flex-direction: column;
}
.geo {
  flex: 1;
  background-color: rgba(255, 255, 255, 0.2);
}

.users {
  height: 1.7604rem;
}

.order {
  // margin-bottom: 0.1042rem;
  height: 0.7813rem;
}

.sales {
  // margin-bottom: 0.1042rem;
  height: 1.2917rem;
}

.wrap {
  display: flex;
}

.channel {
  margin-right: 0.0521rem;
}

.channel,
.quarter {
  height: 1.2344rem;
  flex: 1;
  // margin-bottom: 0.1042rem;
}

.top {
  height: 1.4583rem;
}
.tabs {
  padding: 0.0521rem;
  display: flex;
  align-items: center;

  .line {
    padding: 0 0.1042rem;
    color: #fff;
  }

  span {
    font-size: 0.0938rem;
    cursor: pointer;
    color: aquamarine;
  }
  span.active {
    color: #fff;
  }
}
.container {
  flex: 1;
  overflow: hidden;
}
.content {
  flex: 1;
  overflow: hidden;
}
</style>
