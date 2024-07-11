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
        <div>rtmp_url: </div>
      </div>
    </v-col>
  </v-row>
</template>
<script>

export default {
  data () {
    return {
      loading: true,
      rtmp_url: ''
    }
  },
  async mounted () {
    const res = await fetch(process.env.lambdaUrl + '/start', { method: 'POST' })
    const resjson = await res.json()
    this.rtmp_url = 'https://' + resjson.ingest_endpoint + '/app/' + resjson.stream_key
    this.loading = false
  }
}
</script>
