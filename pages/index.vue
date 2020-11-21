<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <div class="text-center">
      </div>
      <v-card>
        <v-card-title class="headline" v-text="title" />
        <v-row justify="center">
          <v-col cols="10">
            <v-form>
              <v-text-field
              v-model="newPlaylistName"
                :label="labels[0]"
              />
              <v-text-field
                v-model="spreadSheetName"
                :label="labels[1]"
              />
            </v-form>
          </v-col>
        </v-row>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data () {
    return {
      title: 'プレイリスト生成',
      labels: ['新規プレイリスト名', 'スプレッドシートのシート名'],
      newPlaylistName: null,
      spreadSheetName: null
    }
  }
}
</script>
