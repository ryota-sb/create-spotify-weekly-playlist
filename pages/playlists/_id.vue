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
      <v-progress-linear
        height="3"
        indeterminate
        :size="100"
        color="green derken-3"
      ></v-progress-linear>
    </v-row>

    <!-- 新規プレイリスト作成のモーダル画面 -->
    <v-row v-if="isSpotifyPlaylist">
      <v-dialog
        v-model="dialog"
        persistent
        max-width="600px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="green darken-3"
            dark
            fab
            v-bind="attrs"
            v-on="on"
          >
            <v-icon dark>
              mdi-plus
            </v-icon>
          </v-btn>
        </template>
        <v-card>
          <v-row justify="center">
            <v-col cols="10">
              <v-text-field v-model="newPlaylistName" label="新規プレイリスト名"></v-text-field>
            </v-col>
          </v-row>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="blue darken-1"
              text
              @click="dialog = false"
            >
              Close
            </v-btn>
            <v-btn
              color="blue darken-1"
              text
              @click="createPlaylist()"
            >
              Create
            </v-btn>
          </v-card-actions>
          <v-row justify="center">
            <v-progress-linear
              v-if="modalLoading"
              height="3"
              indeterminate
              :size="100"
              color="green darken-3"
            ></v-progress-linear>
          </v-row>
        </v-card>
      </v-dialog>
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      spotify_playlist: [],
      newPlaylistName: null,
      dialog: false,
      modalLoading: false
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
    },
    async createPlaylist() {
      const params = {
        newPlaylistName: this.newPlaylistName,
        spreadSheetName: this.pageId
      }
      const uri = "https://j293sn19j1.execute-api.ap-northeast-1.amazonaws.com/default/create-spotify-playlist"
      this.modalLoading = true
      await this.$axios.$post(uri, params).then(res => {
        this.newPlaylistName = null
        this.dialog = false
        this.modalLoading = false
        console.log(res.data)
      }).catch(err => {
        console.log(err)
      })
    }
  }
}
</script>
