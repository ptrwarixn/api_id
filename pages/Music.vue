<template>
  <div>
    <v-row>
      <v-col cols="10">
        <v-text-field v-model="textsearch" label=""></v-text-field>
      </v-col>
      <v-col cols="2">
        <v-btn @click="getitem()"><v-icon>mdi-magnify</v-icon></v-btn>
      </v-col>
    </v-row>

    <v-row>
      <v-col
        v-for="data in resultData"
        :key="data.trackId"
        class="d-flex child-flex"
        cols="4"
      >
        <nuxt-link :to="{ name: 'id-id', params: { id: data } }">
          <v-hover v-slot:default="{ hover }">
            <v-card flat tile class="d-flex" :elevation="hover ? 12 : 2">
              <v-img
                :src="data.artworkUrl100"
                aspect-ratio="1"
                class="grey lighten-2 rounded"
              >
                <p class="font-weight-bold grey" style="color: white">
                  {{ data.trackName }}
                </p>
              </v-img>
            </v-card>
          </v-hover>
        </nuxt-link>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      resultData: null,
      textsearch: '',
    }
  },
  methods: {
    getitem() {
      axios
        .get(
          'https://itunes.apple.com/search?term=$' +
            this.textsearch +
            '&limit=30'
        )
        .then((res) => {
          this.resultData = res.data.results
          this.$emit('resultData', Object(res.data.results))
        })
        .catch((err) => {
          // eslint-disable-next-line no-console
          console.log(err)
        })
    },
  },
}
</script>

<style></style>
