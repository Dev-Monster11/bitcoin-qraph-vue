<template>
  <div>
    <div class="card-block">
      <InfoView :symbol="value.symbol" :ticker="tickers[value.symbol] || {}"></InfoView>
    </div>
  </div>
</template>
<script>
import CurrencyCard from '../components/CurrencyCard.vue'
import InfoView from './InfoView.vue'
import { mapState } from 'vuex'

export default {
  name: 'CryptoBoard',
  data() {
    return {
      value: { "cid": 1, "symbol": "BTCUSDT", "base": "BTC", "quote": "USDT", "name": "Bitcoin" }
    }
  },
  components: {
    CurrencyCard,
    InfoView
  },
  computed: {
    ...mapState(['tickers', 'currencies'])
  }
}
</script>
