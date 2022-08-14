<route lang="json">
{
  "meta": {
    "title": "Web-App-Scanner"
  }
}
</route>



<template>
    <div class="text-xl">{{ error }}</div>
  <div class="md:container md:mx-auto px-8">
    <p>{{ decodedString }}</p>
    <p>Llevas escaneados<div v-if="decodedString">Llevas escaneados {{ cuenta }}</div></p>
    <div class="">
    <qrcode-stream @init="onInit" @decode="onDecode"></qrcode-stream>
    </div>

  </div>
</template>




<script>
import {QrcodeStream} from 'vue3-qrcode-reader'
export default {
  data() {
    return {
      error: '',
      decodedString: '',
      cuenta: 0,
    }
  },
    components: {
      QrcodeStream
    },
    methods: {
  async onInit (promise) {
    // show loading indicator

    try {
      const { capabilities } = await promise

      // successfully initialized
    } catch (error) {
      if (error.name === 'NotAllowedError') {
        this.error = ("Denegaste el uso de la camara. Para poder usar la aplicacion actualiza y presiona PERMITIR")
      } else if (error.name === 'NotFoundError') {
        this.error = ("No se encontró ninguna cámara instalada.")
      } else if (error.name === 'NotSupportedError') {
        this.error = ("Esta pagina no cuenta con protocolo de seguridad SSL (ni es localhost)")
      } else if (error.name === 'NotReadableError') {
        this.error = ("La camara se encuentra en uso." )
      } else if (error.name === 'OverconstrainedError') {
        this.error = ("did you requested the front camera although there is none?")
      } else if (error.name === 'StreamApiNotSupportedError') {
        this.error = ("browser seems to be lacking features")
      }
      } finally {
      // hide loading indicator
      }
    },
    onDecode(decodedString) {
      this.decodedString = decodedString;
      cuenta++;
    }
  }
}

</script>