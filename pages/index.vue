<template>
  <v-card class="mt-5" flat>
    <v-card-title class="justify-center" primary-title>
      ตารางคะแนนรวม CMRU People Choice Award 2020
    </v-card-title>
    <v-card-subtitle
      >ข้อมูลอัพเดพ: {{ users.time }} <br />หมายเหตุ: การคำนวณคะแนนตาม 1 ไลค์
      ต่อ 1 คะแนน และ 1 แชร์ ต่อ 3 คะแนน</v-card-subtitle
    >
    <v-card-text>
      <!-- Gsci -->
      <v-row justify="center" align="center">
        <v-col cols="12">
          <v-card :loading="tables_loading != true">
            <v-card-title class="justify-center" primary-title>
              Top 3 MISS G-SCI
            </v-card-title>
            <v-divider></v-divider>

            <v-card-text>
              <List :data="ranking.q"></List>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>

      <v-row justify="center" align="center">
        <v-col cols="12">
          <v-card :loading="tables_loading != true">
            <v-card-title class="justify-center" primary-title
              >Top 5 ดาว</v-card-title
            >
            <v-divider></v-divider>
            <v-card-text>
              <List :data="ranking.g"></List>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>

      <v-row justify="center" align="center">
        <v-col cols="12">
          <v-card :loading="tables_loading != true">
            <v-card-title class="justify-center" primary-title
              >Top 5 เดือน</v-card-title
            >
            <v-divider></v-divider>

            <v-card-text>
              <List :data="ranking.b"></List>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>

      <v-row justify="center" align="center">
        <v-col cols="12">
          <v-card>
            <v-card-title class="justify-center" primary-title>
              ตารางคะแนนรวมทั้งหมด
            </v-card-title>
            <v-divider></v-divider>
            <v-card-text>
              <v-data-table
                :headers="tables_headers"
                :items="users.data"
                :loading="tables_loading != true"
                hide-default-footer
                disable-pagination
                mobile-breakpoint="0"
                class="elevation-1"
              >
                <template v-slot:item.action="{ item }">
                  <v-btn :href="item.url.replace('m.', 'www.')" target="_blank">
                    <v-icon>mdi-share</v-icon>
                  </v-btn>
                </template>

                <template v-slot:item.all="{ item }">
                  {{ item.all.toLocaleString() }}
                </template>

                <template v-slot:item.share="{ item }">
                  {{ item.share.toLocaleString() }}
                </template>

                <template v-slot:item.point="{ item }">
                  {{ item.point.toLocaleString() }}
                </template>
              
              </v-data-table>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-card-text>
  </v-card>
</template>

<script>
export default {
  data: function () {
    return {
      selected: null,
      tables_headers: [
        {
          id: 'id',
          align: 'start',
          sortable: false,
          value: 'id',
        },
        { text: 'ชื่อ', value: 'name', sortable: false },
        { text: 'ชื่อเล่น', value: 'nick_name', sortable: false },

        { text: 'สาขา', value: 'class', sortable: false },
        { text: 'จำนวน Like', value: 'all' },
        { text: 'จำนวน Share', value: 'share' },
        { text: 'คะแนนรวม', value: 'point' },
        { text: 'ลิ้ง Url', value: 'action', sortable: false },

        ,
      ],
    }
  },

  mounted: function () {},

  asyncData: async function () {
    try {
      const [users, ranking] = await Promise.all([
        fetch(
          'https://raw.githubusercontent.com/sctnightcore/cmru_People_Choice_Award_2020/master/data.json'
        ).then((res) => res.json()),
        fetch(
          'https://raw.githubusercontent.com/sctnightcore/cmru_People_Choice_Award_2020/master/ranking_data.json'
        ).then((res) => res.json()),
      ])
      return {
        users: users,
        ranking: ranking,
        tables_loading: true,
      }
    } catch (e) {
      if (e.message === 'Network Error') {
        return {
          users: [],
          ranking: [],
          tables_loading: true,
        }
      }
    }
  },
}
</script>