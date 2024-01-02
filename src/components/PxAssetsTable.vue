<template>
  <table>
    <thead>
      <tr
        class="border-b border-purple-400 bg-gradient-to-r from-indigo-600 to-purple-500"
      >
        <th></th>
        <th :class="{ up: this.sortOrder === 1, down: this.sortOrder === -1 }">
          <span
            class="underline cursor-pointer text-gray-300 "
            @click="changeSortOrder"
            >Ranking</span
          >
        </th>
        <th class="text-gray-300">Nombre</th>
        <th class="text-gray-300">Precio</th>
        <th class="text-gray-300">Valor En El Mercado</th>
        <th class="text-gray-300">Variación 24hs</th>
        <td class="hidden sm:block">
          <input
            class=" bg-gray-300 focus:outline-none border-b border-purple-400 py-2 px-4 block w-full appearance-none leading-normal"
            id="filter"
            placeholder="Buscar..."
            type="text"
            v-model="filter"
          />
        </td>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="a in filteredAssets"
        :key="a.id"
        class="border-b border-purple-400 hover:bg-indigo-600 "
      >
        <td>
          <img
            class="w-6 h-6"
            :src="
              `https://static.coincap.io/assets/icons/${a.symbol.toLowerCase()}@2x.png`
            "
            :alt="a.name"
          />
        </td>
        <td>
          <b class="text-gray-300"># {{ a.rank }}</b>
        </td>
        <td class="text-gray-300">
          <router-link
            class="hover:underline text-white"
            :to="{ name: 'coin-detail', params: { id: a.id } }"
            >{{ a.name }}</router-link
          >
          <small class="ml-1 text-gray-400">{{ a.symbol }}</small>
        </td>
        <td class="text-gray-300">{{ a.priceUsd | dollar }}</td>
        <td class="text-gray-300">{{ a.marketCapUsd | dollar }}</td>
        <td
          :class="
            a.changePercent24Hr.includes('-')
              ? 'text-red-500'
              : 'text-green-400'
          "
        >
          {{ a.changePercent24Hr | percent }}
        </td>
        <td class="hidden sm:block">
          <px-button @custom-click="goToCoin(a.id)">
            <span>Detalle</span>
          </px-button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
import PxButton from '@/components/PxButton'

export default {
  name: 'PxAssetsTable',

  components: { PxButton },

  data() {
    return {
      filter: '',
      sortOrder: 1
    }
  },

  props: {
    assets: {
      type: Array,
      default: () => []
    }
  },

  computed: {
    filteredAssets() {
      const altOrder = this.sortOrder === 1 ? -1 : 1

      return this.assets
        .filter(
          a =>
            a.symbol.toLowerCase().includes(this.filter.toLowerCase()) ||
            a.name.toLowerCase().includes(this.filter.toLowerCase())
        )
        .sort((a, b) => {
          if (parseInt(a.rank) > parseInt(b.rank)) {
            return this.sortOrder
          }

          return altOrder
        })
    }
  },

  methods: {
    goToCoin(id) {
      this.$router.push({ name: 'coin-detail', params: { id } })
    },

    changeSortOrder() {
      this.sortOrder = this.sortOrder === 1 ? -1 : 1
    }
  }
}
</script>

<style scoped>
.up::before {
  content: '👆';
}

.down::before {
  content: '👇';
}

td {
  padding: 20px 0px;
  font-size: 0.6rem;
  text-align: center;
}

th {
  padding: 5px;
  font-size: 0.6rem;
}

@media (min-width: 640px) {
  td,
  th {
    padding: 20px;
    font-size: 1rem;
  }

  th {
    padding: 12px;
  }
}
</style>
