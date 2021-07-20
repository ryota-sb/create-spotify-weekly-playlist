<template>
  <div>
    <!-- ワークシートタイトル取得後、一覧表示 -->
    <v-row dense v-if="isWorksheetTitleList">
      <v-col cols="4" md="4" v-for="(worksheet_title, i) in worksheet_title_list" :key="i">
        <v-card :to="{ name: 'playlists-id', params: { id: worksheet_title } }">
          <v-card-title>{{ worksheet_title }}</v-card-title>
        </v-card>
      </v-col>
    </v-row>

    <!-- ワークシートタイトル取得までのロード画面 -->
    <v-row v-else align="center" justify="center">
      <v-progress-circular
        height="500"
        indeterminate
        :size="100"
        color="grey lighten-5"
      ></v-progress-circular>
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      worksheet_title_list: []
    }
  },
  computed: {
    isWorksheetTitleList() {
      return this.worksheet_title_list.length
    }
  },
  created() {
    this.getWorksheetTitle()
  },
  methods: {
    async getWorksheetTitle() {
      const uri = "https://sxqsmtor2i.execute-api.ap-northeast-1.amazonaws.com/default/getspotifyplaylistworksheettitleapi"
      await this.$axios.$get(uri).then(res =>{
        this.worksheet_title_list = res
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>
