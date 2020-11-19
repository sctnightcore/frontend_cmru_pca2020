<template>
  <v-list two-line>
    <v-list>
      <v-list-item v-for="(item, index) in data" :key="item.id">
        <v-hover v-slot:default="{ hover }">
          <v-list-item :key="item.name">
            <template v-slot:default="{ active, toggle }">
              <v-expand-transition>
                <v-overlay absolute :opacity="0.9" :value="hover">
                  <v-icon>mdi-heart</v-icon>
                  <span class="white--text">{{ item.all.toLocaleString() }}</span>
                  <v-icon>mdi-share</v-icon>
                  <span class="white--text">{{ item.share.toLocaleString() }}</span>
                  <v-icon>mdi-star</v-icon>
                  <span class="white--text">{{ item.point.toLocaleString() }}</span>
                </v-overlay>
              </v-expand-transition>
              <v-list-item-avatar tile>
                {{ index + 1 }}
              </v-list-item-avatar>

              <v-list-item-content class="align-self-start">
                <v-list-item-title v-text="item.name"></v-list-item-title>
                <v-list-item-subtitle
                  class="text--primary"
                  v-text="item.class"
                ></v-list-item-subtitle>
              </v-list-item-content>
            </template>
          </v-list-item>
        </v-hover>
      </v-list-item>
    </v-list>
  </v-list>
</template>

<script>
export default {
  props: ['data'],
  data: function () {
    return {
      selected: null,
      lockSelection: false,
    }
  },
}
</script>

<style scoped>
.span {
  color: black
}
</style>