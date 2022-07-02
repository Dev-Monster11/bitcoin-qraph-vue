<template>
  <div class="container-fluid">
    <!-- <div class="row flex-xl-nowrap">
            <div class="col"> -->
    <div class="row">
      <div class="col-md-12 mb-3 mt-3">
        <div class="info-card">
          <div class="row">
            <div class="col">
              <span class="coin-img"
                :style="{ backgroundImage: `url('https://s2.coinmarketcap.com/static/img/coins/64x64/${currency.cid}.png')` }"></span>
              <div class="coin-name">{{ currency.name }} ({{ currency.base }}) / <span
                  class="small">{{ currency.quote }}</span></div>
            </div>
          </div>
          <div class="row">
            <div class="col-md-6">
              <span class="price"
                :style="{ 'color': (ticker.pchg && ticker.pchg > 0) ? 'green' : 'red' }">{{ ticker.price }}<span
                  class="x-small"> {{ currency.quote }}</span></span>
            </div>
            <div class="col-md-6 chg-block" :class="[(ticker.percent < 0) ? 'down' : 'up']">
              <div class="text-dark small text-right">24h Chg</div>
              <div class="text-right d-flex justify-content-end">
                <span class="indicator"></span><span>{{ ticker.percent }}%</span>
              </div>
              <div class="icon-chg text-right">
                {{ parseFloat(ticker.chg).toFixed((currency.quote === 'USDT') ? 3 : 8) }} <span
                  class="x-small">{{ currency.quote }}</span>
              </div>
            </div>
            <div class="col-12 x-small">{{ ticker.time | timeformat }}</div>
          </div>
        </div>
      </div>
      <div class="col-12 col-md-12" v-if="ticker.price">
        <coin-charts :symbol="symbol"></coin-charts>
        <Sparkline :cdata="ticker.price" :width="380" :height="90"></Sparkline>
      </div>
    </div>
  </div>
</template>
<script>
import CoinCharts from '../components/CoinCharts.vue'
import Sparkline from '../components/Sparkline.vue';
const months = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
export default {
  props: ['symbol'],
  name: 'info-view',
  data() {
    return {

    };
  },
  filters: {
    priceformat: function (value) {
      if (!value) return "";
      return parseFloat(value).toLocaleString()
    },
    timeformat: function (value) {
      if (!value) return "";
      const dt = new Date(value);
      return `${dt.getDate()} ${months[dt.getMonth()]} ${dt.toTimeString().split(' ')[0]}`
    }
  },
  components: {
    CoinCharts,
    Sparkline
  },
  computed: {
    currency() {
      return this.$store.getters.getSymbolById(this.symbol) || {}
    },
    ticker() {
      return this.$store.getters.getTickerById(this.symbol) || {}
    }
  }
}
</script>
