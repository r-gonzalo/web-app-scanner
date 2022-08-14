<route lang="json">
{
  "meta": {
    "title": "Web-App-Scanner"
  }
}
</route>


<template>
  <div>
    <p class="decode-result">Last result: <b>{{ result }}</b></p>
    <p> Total: <b> {{ total }}</b></p>

    <qrcode-stream :camera="camera" @decode="onDecode" @init="onInit">
      <div v-show="showScanConfirmation" class="scan-confirmation">
        <img :src="$withBase('/checkmark.svg')" alt="Checkmark" width="128px" />
      </div>
    </qrcode-stream>
  </div>
</template>

<script>
import { QrcodeStream } from 'vue3-qrcode-reader'

export default {

  components: { QrcodeStream },

  data () {
    return {
      camera: 'auto',
      result: null,
      showScanConfirmation: false,
      total: 0,
    }
  },

  methods: {

    async onInit (promise) {
      try {
        await promise
      } catch (e) {
        console.error(e)
      } finally {
        this.showScanConfirmation = this.camera === "off"
      }
    },

    async onDecode (content) {
      this.result = content

      this.pause()
      await this.timeout(500)
      this.unpause()
      this.sumador()

    },

    unpause () {
      this.camera = 'auto'
    },

    pause () {
      this.camera = 'off'
    },
    contador () {
      return this.total++
    },

    timeout (ms) {
      return new Promise(resolve => {
        window.setTimeout(resolve, ms)
      })
    }
  }
}
</script>

<style scoped>
.scan-confirmation {
  position: absolute;
  width: 100%;
  height: 100%;

  background-color: rgba(255, 255, 255, .8);

  display: flex;
  flex-flow: row nowrap;
  justify-content: center;
}
</style>