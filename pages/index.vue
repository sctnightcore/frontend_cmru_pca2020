<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="12" md="12">
      <v-card>
        <v-card-title class="headline justify-center" primary-title>
          ตารางคะแนนรวม ของ CMRU People Choice Award 2020
        </v-card-title>
        <v-divider> </v-divider>
        <v-card-text>
          <v-data-table
            :headers="tables_headers"
            :items="users"
            :loading="tables_loading != true"
            hide-default-footer
            disable-pagination
            class="elevation-1 pa-2 ma-2"
          ></v-data-table>
          <!-- Show Chart-->
          <Chart :charts="charts" :height="200"></Chart>
        </v-card-text>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import Chart from '~/components/Chart.vue'
export default {
  components: { Chart },
  data() {
    return {
      tables_headers: [
        {
          id: 'id',
          align: 'start',
          sortable: false,
          value: 'id',
        },
        { text: 'ชื่อ', value: 'name' },
        { text: 'สาขา', value: 'class' },
        { text: 'จำนวน Like', value: 'like' },
        { text: 'จำนวน Share', value: 'share' },
        { text: 'คะแนนรวม', value: 'point' },
        ,
      ],
    }
  },
  async asyncData({ $axios }) {
    const [users, charts] = await Promise.all([
      $axios.get(
        'https://raw.githubusercontent.com/sctnightcore/cmru_People_Choice_Award_2020/master/data.json'
      ),
      $axios.get(
        'https://raw.githubusercontent.com/sctnightcore/cmru_People_Choice_Award_2020/master/chart_data.json'
      ),
    ])

    return {
      users: users.data,
      charts: charts.data,
      tables_loading: true,
    }
  },

  mounted() {},
}
</script>
