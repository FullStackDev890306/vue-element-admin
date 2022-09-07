<template>
  <div class="flex">
    <div>
      <h2 class="text-center">
        Top Gainers
      </h2>
      <el-table :data="topGainers" style="width: 100%;1padding-top: 15px;">
        <el-table-column label="No" width="50">
          <template slot-scope="scope">
            {{ scope.$index + 1 }}
          </template>
        </el-table-column>
        <el-table-column label="Name" width="150" align="center">
          <template slot-scope="scope">
            <image :src="scope.row.logo" style="width: 20px; height: 20px; margin-right: 5px;" />
            {{ scope.row.name }}
          </template>
        </el-table-column>
        <el-table-column label="Price" width="150" align="center">
          <template slot-scope="scope">
            {{ scope.row.dayChangeAmount }}
          </template>
        </el-table-column>
        <el-table-column label="24h Change" width="150" align="center">
          <template slot-scope="scope">
            +{{ scope.row.dayChange }}%
          </template>
        </el-table-column>
      </el-table>
    </div>

    <div>
      <h2 class="text-center">
        Top Losers
      </h2>
      <el-table :data="topLosers" style="width: 100%;1padding-top: 15px;">
        <el-table-column label="No" width="50">
          <template slot-scope="scope">
            {{ scope.$index + 1 }}
          </template>
        </el-table-column>
        <el-table-column label="Name" width="150" align="center">
          <template slot-scope="scope">
            <image :src="scope.row.logo" style="width: 20px; height: 20px; margin-right: 5px;" />
            {{ scope.row.name }}
          </template>
        </el-table-column>
        <el-table-column label="Price" width="150" align="center">
          <template slot-scope="scope">
            {{ scope.row.dayChangeAmount }}
          </template>
        </el-table-column>
        <el-table-column label="24h Change" width="150" align="center">
          <template slot-scope="scope">
            {{ scope.row.dayChange }}%
          </template>
        </el-table-column>
      </el-table>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        success: 'success',
        pending: 'danger'
      }
      return statusMap[status]
    },
    orderNoFilter(str) {
      return str.substring(0, 30)
    }
  },
  data() {
    return {
      topGainers: null,
      topLosers: null
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      axios.get('https://www.binance.com/bapi/composite/v1/public/marketing/symbol/list').then(res => {
        // filter res.data.data.tag include 'pos'
        const dat = res.data.data.filter(item => !item.tags.includes('ETF'))
        dat.sort((a, b) => {
          return b.dayChange - a.dayChange
        })
        this.topGainers = dat.slice(0, 10)
        this.topLosers = dat.slice(-10).reverse()
      })
    }
  }
}
</script>

<style>
  .flex {
    display: flex;
    justify-content: space-around;
  }
</style>
