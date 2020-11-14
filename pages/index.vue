<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="12" md="12">
      <v-card>
        <v-card-title class="justify-center" primary-title>
          ตารางคะแนนรวม CMRU People Choice Award 2020
        </v-card-title>
        <v-divider> </v-divider>
        <v-card-text class="pa-2 ma-2">
          <h3>อัพเดพข้อมูลล่าสุด {{ users.time }}</h3>
          <v-data-table
            :headers="tables_headers"
            :items="users.data"
            :loading="tables_loading != true"
            hide-default-footer
            disable-pagination
          >
            <template v-slot:item.action="{ item }">
              <v-btn :href="item.url.replace('m.', 'www.')">
                <v-icon>mdi-share</v-icon>
              </v-btn>
            </template>
          </v-data-table>
          <br/>
          <!-- Show Chart-->
          <Chart :charts="charts.data" :width="500" :height="300"></Chart>
        </v-card-text>
        <v-card-actions>
          <v-icon x-large left>mdi-github</v-icon>
          <h4>
            โปรเจคนี้ อยู่ภายใต้ GNU General Public License v3.0 ว่าด้วย Open
            Source&nbsp
          </h4>
          <v-btn
            href="https://github.com/sctnightcore/frontend_cmru_pca2020/blob/main/LICENSE"
            >คลิ๊กที่นี้เพื่ออ่าน</v-btn
          >
        </v-card-actions>
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
        { text: 'ชื่อ', value: 'name', sortable: false },
        { text: 'สาขา', value: 'class', sortable: false },
        { text: 'ลิ้ง Url', value: 'action', sortable: false },
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
