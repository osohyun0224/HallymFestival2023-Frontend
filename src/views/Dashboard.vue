<template>
  <main>
    <div class="introment">
      <p>"과거에서 일어나는 우리의 설레는 이야기 ..."</p>
    </div>
    <div class="logo">
      <img src="@/assets/로고.png" alt="" width="400" height="300" />
    </div>
    <div class="totalvisitor">
      <h3>Total</h3>
      <h1>{{ total.toLocaleString() }}</h1>
      <h5 class="today">Today</h5>
      <h2>{{ today.toLocaleString() }}</h2>

      <div class="chart">
        <Chart :options="chart"></Chart>
      </div>
    </div>
    <div class="ownermenu" ref="ownermenu">
      <RouterLink to="/announcement">공지사항</RouterLink>
      <RouterLink to="/boothmap">부스 목록</RouterLink>
      <RouterLink to="/admin/comment">신고받은 댓글 목록</RouterLink>
      <RouterLink to="/admin/visitcomment">신고받은 방명록 목록</RouterLink>
    </div>
  </main>
</template>

<script>
import { Chart } from 'highcharts-vue';
import { GetVisitorCount } from '../api/api-client';

export default {
  components: {
    Chart
  },

  data() {
    return {
      today: 0,
      total: 0,
      chart: {
        chart: {
          backgroundColor: '#333333',
          style: { fontFamily: 'NanumGothic, Helvetica, Arial, sans-serif' },
          spacing: [0, 20, 20, 20],
          height: 300
        },
        credits: {
          enabled: false
        },
        xAxis: {
          categories: [],
          labels: {
            style: { fontSize: '9pt', color: '#ffffff' }
          }
        },
        yAxis: {
          visible: false
        },
        legend: {
          enabled: false
        },
        series: [
          {
            name: '방문자',
            data: []
          }
        ]
      }
    };
  },

  props: {},

  async created() {
    const data = await GetVisitorCount();

    this.total = data.reduce((result, item) => result + item.count, 0);
    this.today = data[0].count;

    data.forEach((item) => (item.visitDate = parseInt(item.visitDate.split('-').pop()) + '일'));
    let length = data.length;
    for (let i = 0; i < 7 - length; i++) {
      data.push({
        visitDate: '-',
        count: 0
      });
    }

    this.chart.xAxis.categories = data
      .slice(0, 7)
      .reverse()
      .map((item) => item.visitDate);
    this.chart.series[0].data = data
      .slice(0, 7)
      .reverse()
      .map((item) => item.count);
  }
};
</script>

<style scoped>
main {
  margin-bottom: 80px;
}

.logo {
  width: 40%;
  margin: -18px auto;
  display: flex;
  justify-content: center;
}

.introment {
  margin-top: 36px;
  font-family: 'Nanum Gothic', sans-serif;
  font-style: normal;
  font-size: 15px;
  line-height: 13px;
  text-align: center;
  letter-spacing: 0.15em;
  padding: 36px 0;
  color: #ffffff;
}

.today {
  margin-top: 18px;
}

.totalvisitor {
  text-align: center;
  margin: 0;
  padding: 10px 0;
  color: #ffffff;
}

.chart {
  margin: auto;
  margin-top: 16px;
  max-width: 400px;
  height: 300px;
  overflow: hidden;
}
.ownermenu {
  font-family: 'Nanum Gothic', sans-serif;
  max-width: 322.29px;
  margin: auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  margin-top: 24px;
}
.ownermenu > * {
  width: 100%;
  margin: 4px;
  padding: 10px 0;
  border-radius: 46.25px;
  font-size: 13pt;
  text-align: center;
  text-decoration: none;
  color: black;
  background-color: lightgray;
}
</style>
