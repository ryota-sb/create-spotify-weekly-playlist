<template>
  <div>
    <!-- プレイリスト取得後、一覧表示 -->
    <v-row dense v-if="isSpotifyPlaylist">
      <v-col v-for="(image, i) in spotify_playlist[1]['playlist_image']" :key="i" cols="4" md="4">
        <v-card>
          <div class="d-flex flex-no-wrap">
            <v-img :src="image" max-width="100" max-height="100" />
            <v-card-title v-text="spotify_playlist[0]['playlist_name'][i]" />
          </div>
        </v-card>
      </v-col>
    </v-row>

    <!-- プレイリスト取得までのロード画面 -->
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
      spotify_playlist: [],
    }
  },
  computed: {
    pageId() {
      return this.$route.params.id
    },
    isSpotifyPlaylist() {
      return this.spotify_playlist.length
    }
  },
  created() {
    this.getSpotifyPlaylistData()
  },
  methods: {
    async getSpotifyPlaylistData() {
      const uri = "https://3wz93invpk.execute-api.ap-northeast-1.amazonaws.com/default/getspotifyplaylistdataapi"
      const params = { worksheetTitle: this.pageId }
      await this.$axios.$post(uri, params).then(res => {
        this.spotify_playlist = res
        console.log(this.spotify_playlist)
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>
