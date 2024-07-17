<template>
  <v-row
    justify="center"
    align="center"
  >
    <v-col cols="12">
      <v-card>
        <video
          id="video-player"
          class="video-js vjs-fluid vjs-big-play-centered"
          controls
          playsinline
          autoplay
        />
      </v-card>
    </v-col>
    <v-col cols="12">
      <v-text-field v-model="text" label="text here" />
      <v-btn color="success" @click="onSend">
        Send
      </v-btn>
      <v-list>
        <v-list-item v-for="(message, i) in messages" :key="i">
          {{ message }}
        </v-list-item>
      </v-list>
    </v-col>
  </v-row>
</template>
<script>
import videojs from 'video.js'

export default {
  data () {
    return {
      ivs: null,
      arn: this.$route.params.arn,
      streamUrl: null,
      player: null,
      timestamp: new Date().getTime(),
      messages: [],
      websocket: null,
      text: ''
    }
  },
  async mounted () {
    const res = await fetch(process.env.LAMBDA_URL + '/stream?arn=' + this.$route.params.arn)
    const resjson = await res.json()
    this.streamUrl = resjson.playback_url
    if (this.ivs === null) {
      this.ivs = require('amazon-ivs-player')
      this.ivs.registerIVSTech(videojs, {
        wasmBinary: '/_nuxt/amazon-ivs-wasmworker.min.wasm',
        wasmWorker: '/_nuxt/amazon-ivs-wasmworker.min.js'
      })
      this.ivs.registerIVSQualityPlugin(videojs)
    }
    const chatToken = resjson.chat_token
    this.websocket = new WebSocket('wss://edge.ivschat.ap-northeast-1.amazonaws.com', chatToken)
    this.websocket.onmessage = (event) => {
      if (!event.data) { return }
      this.messages.unshift(JSON.parse(event.data).Content)
    }

    this.startStream()
  },
  beforeDestroy () {
    this.player.dispose()
  },
  methods: {
    startStream () {
      this.player = videojs('video-player', {
        techOrder: ['AmazonIVS']
      })
      this.player.enableIVSQualityPlugin()
      this.player.src(this.streamUrl)
    },
    onSend () {
      const payload = {
        Action: 'SEND_MESSAGE',
        Content: this.text
      }
      this.websocket.send(JSON.stringify(payload))
      this.text = ''
    }
  }
}
</script>
