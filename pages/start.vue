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
        <div>rtmp_url: {{ rtmp_url }}</div>
        <div>ffmpeg例: ffmpeg -stream_loop -1 -re -i （ファイル名）.mp4 -vf "scale=854:480" -c:v libx264 -preset veryfast -b:v 1500k -c:a aac  -f flv {{ rtmp_url }}</div>
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
    const res = await fetch(process.env.LAMBDA_URL + '/start', { method: 'POST' })
    const resjson = await res.json()
    this.rtmp_url = 'rtmp://' + resjson.ingest_endpoint + '/app/' + resjson.stream_key
    this.loading = false
  }
}
</script>
