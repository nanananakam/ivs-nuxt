<template>
  <v-row
    justify="center"
    align="center"
  >
    <v-col cols="12">
      <div v-if="loading">
        <v-progress-linear indeterminate color="primary" />
      </div>
      <div v-if="!loading">
        <div v-for="stream in streams">
          <nuxt-link :to="{ name: 'watch-arn', params: { 'arn': stream } }">
            {{ stream }}
          </nuxt-link>
        </div>
      </div>
    </v-col>
  </v-row>
</template>
<script>

export default {
  data () {
    return {
      loading: true,
      streams: []
    }
  },
  async mounted () {
    const res = await fetch(process.env.LAMBDA_URL + '/streams')
    const resjson = await res.json()
    this.streams = resjson.arns
    this.loading = false
  }
}
</script>
