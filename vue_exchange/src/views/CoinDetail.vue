<template>
  <div class="flex-col">
    <div class="flex justify-center">
      <bounce-loader :loading="isLoading" :color="'#68d391'" :size="100" /><!--componente de vue-spiner -->
    </div>
    <div class="flex justify-center">
    </div>
    <template v-if="!isLoading"> <!-- v-if="!isLoading" se muestra si es false -->
      <div class="flex flex-col sm:flex-row justify-around items-center">
        <div class="flex flex-col items-center">
          <img
            :src="
              `https://static.coincap.io/assets/icons/${asset.symbol.toLowerCase()}@2x.png`
            "
            :alt="asset.name"
            class="w-20 h-20 mr-5"
          />
          <h1 class="text-5xl">
              {{ asset.name }}
            <small class="sm:mr-2 text-gray-500">{{ asset.symbol }}</small>
          </h1>
        </div>

        <div class="my-10 flex flex-col">
          <ul>
            <li class="flex justify-between">
              <b class="text-gray-600 mr-10 uppercase">Ranking</b>
              <span># {{ asset.rank }}</span>
            </li>
            <li class="flex justify-between">
              <b class="text-gray-600 mr-10 uppercase">Precio actual</b>
              <span>{{ asset.priceUsd | dollar }}</span>
            </li>
            <li class="flex justify-between">
              <b class="text-gray-600 mr-10 uppercase">Precio más bajo</b>
              <span>{{ min | dollar}}</span>
            </li>
            <li class="flex justify-between">
              <b class="text-gray-600 mr-10 uppercase">Precio más alto</b>
              <span>{{ max | dollar }}</span>
            </li>
            <li class="flex justify-between">
              <b class="text-gray-600 mr-10 uppercase">Precio Promedio</b>
              <span>{{ avg | dollar }}</span>
            </li>
            <li class="flex justify-between">
              <b class="text-gray-600 mr-10 uppercase">Variación 24hs</b>
              <span>{{ asset.changePercent24Hr | percent }}</span>
            </li>
          </ul>
        </div>
      </div>

      <line-chart class="my-10"
        :colors="['orange']"
        :min="min"
        :max="max"
        :data="history.map(h => [h.date, parseFloat(h.priceUsd).toFixed(2)])"
      /><!-- componente de vue-chartkic-->

      <h3 class="text-xl my-10">Mejores Ofertas de Cambio</h3>
      <table>
        <tr v-for="m in markets" :key="`${m.exchangeId}-${m.priceUsd}`" class="border-b">
          <td>
            <b>{{ m.exchangeId }}</b>
          </td>
          <td>{{  m.priceUsd | dollar }}</td>
          <td>{{ m.baseSymbol }} / {{ m.quoteSymbol }}</td>
          <td>
            <art-button
              :is-loading = "m.isLoading || false"
              v-if = "!m.url"
              @custom-click = "getWebsite(m)">
              <slot>Obtener Link</slot>
            </art-button>

            <a v-else class="hover:underline text-green-600" target="_blank">
              {{ m.url }}
            </a>
          </td>
        </tr>
      </table>
    </template>
  </div>
</template>

<script>
import ArtButton from '../components/ArtButton.vue'
import api from '@/api'

export default {
  components: { ArtButton },
    name: 'CoinDetail',

    data() {
        return {
          isLoading: false, 
          asset: {},
          history: [],
          markets: []
        }
    },

    computed: {
        min() { 
            return Math.min(
                ...this.history.map(h => parseFloat(h.priceUsd).toFixed(2))
            )
        },

        max() {
            return Math.max(
                ...this.history.map(h => parseFloat(h.priceUsd).toFixed(2))
            )
        },
        
        avg() {
            return Math.abs(
                ...this.history.map(h => parseFloat(h.priceUsd).toFixed(2))
            )
        }
    },

    created() {
        this.getCoin()
    },

    methods: {
        getCoin() { //obtener la informacion de la api y ser ejecutada en created()
            const id = this.$route.params.id    //$rout accede a las rutas y asi al id
            this.isLoading = true

            Promise.all([
                  api.getAsset(id), 
                  api.getAssetHistory(id),
                  api.getMarkets(id)
                ]).then(
                ([asset, history, markets]) => {
                    this.asset = asset
                    this.history = history
                    this.markets = markets
                }
            )
            .finally(() => this.isLoading = false)
        },
      
      getWebsite(exchange){
        this.$set(exchange, 'isLoading', true)
        return api.getExchange(exchange.exchangeId).then(res => {
          this.$set(exchange, 'url', res.exchangeUrl)  //corrigiendo problemas de reactividad, de objetos o array de cosas que se agregan luegos
        }).finally(() => {
            this.$set(exchange, 'isLoading', false)  //se cambia el isLoading
        })
      }
    }

}
</script>

<style scoped>
td {
  padding: 10px;
  text-align: center;
}
</style>