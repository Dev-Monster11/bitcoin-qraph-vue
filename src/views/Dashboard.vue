<template>
    <div class="content-box">
        <CryptoBoard></CryptoBoard>
    </div>
</template>
<script>
    import vSelect from 'vue-select'
    import coins from '@/assets/group.json'
    import CryptoBoard from '@/views/CryptoBoard.vue'
    import {isEmpty} from '../util/Utility'
    import {subscribeSymbol} from '../services/binance'
    import {mapState} from 'vuex'

    export default {
        name: 'dashboard',
        data() {
            return {
                currencyList: coins,
                quote: 'BNB',
                baseCurrency: {}
            }
        },
        mounted() {
            if (this.currencies) {
                this.currencies.forEach(currency => {
                    subscribeSymbol(currency.symbol);
                });
            }
        },
        computed: {
            ...mapState(['currencies']),
            quoteOptions() {
                return Object.keys(coins)
            }
        },
        components: {
            vSelect,
            CryptoBoard
        },
        methods: {
            resetBase() {
                this.baseCurrency = {}
            },
            clear() {
                localStorage.clear();
                location.reload();
            },
            addCoinPair() {
                if (!isEmpty(this.baseCurrency)) {
                    const symbol = `${this.baseCurrency.value}${this.quote}`;
                    subscribeSymbol(symbol);
                    this.$store.commit('ADD_COIN_PAIR', {
                        "symbol": symbol,
                        "base": this.baseCurrency.value,
                        "quote": this.quote,
                        "name": this.baseCurrency.name,
                        "cid": this.baseCurrency.cid
                    })
                }
            }
        }
    }
</script>
