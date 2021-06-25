<template>
  <q-layout view="lHh Lpr lFf">
    <q-page class="flex flex-center">
      <div class="flex flex-center">
      <img
        alt="Quasar logo"
        src="~assets/sh.png"
      >
        <q-btn
          size="30px"
          class="q-px-xl"
          color="secondary"
          label="Ativar Leitor"
          @click="ler"
        />
      </div>
    </q-page>
  </q-layout>
</template>

<script>

// document.addEventListener('deviceready', onDeviceReady, false);

import { defineComponent } from 'vue';
import { useQuasar } from 'quasar';

export default defineComponent({
  name: 'MainLayout',

  setup() {
    const $q = useQuasar();

    function exibir(result) {
      $q.dialog({
        title: 'Resultado',
        message: `Conteúdo: ${result.text}  ----- Formato:${result.format}`,
        color: 'secondary',
      });
    }
    function ler() {
      cordova.plugins.barcodeScanner.scan(
        (result) => {
          exibir(result);
          /* alert(`${'Leitura Realizada\n'
                + 'Resultado: '}${result.text}\n`
                + `Formato: ${result.format}\n`); */
        },
        (error) => {
          alert(`Scanning failed: ${error}`);
        },
        {
          preferFrontCamera: false, // iOS and Android
          showFlipCameraButton: true, // iOS and Android
          showTorchButton: true, // iOS and Android
          torchOn: false, // Android, launch with the torch switched on (if available)
          saveHistory: true, // Android, save scan history (default false)
          prompt: 'Insira o código na área do scan', // Android
          // eslint-disable-next-line max-len
          resultDisplayDuration: 500, // Android, display scanned text for X ms. 0 suppresses it entirely, default 1500
          formats: 'QR_CODE,PDF_417,EAN_13,DATA_MATRIX, UPC_A, EAN_8, CODE_39, CODE_93, CODE_128, CODABAR', // default: all but PDF_417 and RSS_EXPANDED
          orientation: 'landscape', // Android only (portrait|landscape), default unset so it rotates with the device
          disableAnimations: true, // iOS
          disableSuccessBeep: false, // iOS and Android
        },
      );
    }
    return {
      ler,
      exibir,
    };
  },
});
</script>

<style scoped>
</style>
