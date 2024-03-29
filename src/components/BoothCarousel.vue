<template>
  <div>
    <Carousel :items-to-show="1" :wrapAround="true" v-model="_slide">
      <Slide v-if="isAdmin" v-for="(item, index) in charts" :key="index">
        <Chart :options="item"></Chart>
      </Slide>
      <Slide v-if="!isAdmin" v-for="(item, index) in image" :key="index">
        <Image class="image" :src="item" :width="400" :height="400" :spinner-size="300" />
      </Slide>

      <template #addons>
        <Navigation />
      </template>
    </Carousel>
  </div>
</template>

<script>
import { Chart } from 'highcharts-vue';
import { Carousel, Slide, Navigation } from 'vue3-carousel';
import Image from '../components/Image.vue';

import BoothMap1Image from '@/assets/boothmap/1.png';
import BoothMap2Image from '@/assets/boothmap/2.png';
import BoothMap3Image from '@/assets/boothmap/3.png';
import BoothMap4Image from '@/assets/boothmap/4.png';

import {
  GetBoothListWithComment,
  GetBoothListWithLike,
  GetBoothListWithReport
} from '../api/api-client';

function CreateChart(title, item, list) {
  return {
    chart: {
      type: 'bar',
      backgroundColor: '#333333',
      style: { fontFamily: 'NanumGothic, Helvetica, Arial, sans-serif' },
      spacing: [20, 10, 20, 20]
    },
    title: {
      text: title,
      style: { color: '#ffffff' },
      align: 'left'
    },
    credits: {
      enabled: false
    },
    xAxis: {
      categories: list.map((item) => item[0]),

      labels: {
        x: 0,
        y: -25,
        align: 'left',
        formatter: function () {
          if (this.value.length > 15) {
            return this.value.slice(0, 15) + '...';
          } else {
            return this.value;
          }
        },
        style: { color: '#ffffff', whiteSpace: 'nowrap' }
      }
    },
    yAxis: {
      visible: false,
      title: {
        text: item,
        align: 'high'
      },
      labels: {
        style: { color: '#ffffff' },
        overflow: 'justify'
      },
      gridLineWidth: 0
    },
    plotOptions: {
      bar: {
        borderColor: 'none',
        borderRadius: 60,
        dataLabels: {
          enabled: true,
          align: 'right',
          x: -10,
          color: '#333333',
          style: {
            textOutline: 0
          }
        }
      }
    },
    legend: {
      enabled: false
    },
    tooltip: {
      enabled: false
    },
    series: [
      {
        data: list.map((item) => item[1])
      }
    ]
  };
}

export default {
  components: {
    Carousel,
    Slide,
    Navigation,
    Image,
    Chart
  },

  data() {
    return {
      _slide: 0,

      charts: [],

      image: [BoothMap1Image, BoothMap2Image, BoothMap3Image, BoothMap4Image]
    };
  },

  props: {
    isAdmin: {
      type: Boolean,
      default: false
    },
    slide: {
      type: Number,
      default: 0
    }
  },

  computed: {
    chart() {}
  },

  watch: {
    slide(value) {
      this._slide = value;
    }
  },

  async created() {
    if (this.isAdmin) {
      const commentList = (await GetBoothListWithComment()).dtoList;
      const likeList = (await GetBoothListWithLike()).dtoList;
      const reportList = (await GetBoothListWithReport()).dtoList;

      let data;

      data = commentList.map((item) => [item.booth_title, item.comment_cnt]).slice(0, 5);

      this.charts.push(CreateChart('댓글이 가장 많은 부스 TOP 5', '댓글', data));

      data = likeList.map((item) => [item.booth_title, item.like_cnt]).slice(0, 5);

      this.charts.push(CreateChart('좋아요가 가장 많은 부스 TOP 5', '좋아요', data));

      data = reportList.map((item) => [item.booth_title, item.report_cnt]).slice(0, 5);

      this.charts.push(CreateChart('신고받은 댓글이 가장 많은 부스 TOP 5', '신고받은 댓글', data));
    }
  }
};
</script>
<style scoped>
div {
  width: 100%;
  margin-left: -6px;
}
:deep(.carousel) {
  max-width: 420px;
  margin: auto;
}

:deep(.image) {
  width: 100%;

  object-fit: contain;
}

:deep(.carousel__prev),
:deep(.carousel__next) {
  color: white;
}

:deep(.highcharts-container) {
  width: 100%;
  height: 300px;
}
</style>
