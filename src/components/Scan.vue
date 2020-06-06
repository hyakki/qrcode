<template>
  <div class="scan">
    <h1>1.Scan</h1>
    <button class="scan__btn btn btn-info"
            @click.prevent="onScan($event)">
      Decode
    </button>
    <div class="scan__video-outer"
         v-if="videoVisible">
      <video class="scan__video" id="video"></video>
    </div>
    <div class="scan__results"
         v-if="barcodeResult">
      <div class="scan__results__headline">
        Votre codebarre a ete identifie
      </div>
      <div class="scan__results__result"
           v-html="barcodeResult">
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, ref } from "@vue/composition-api";
import { BrowserBarcodeReader } from "@zxing/library";

const codeReader = new BrowserBarcodeReader();

export default defineComponent({
  name: "scan",
  setup() {
    const barcodeResult = ref(null)
    const videoVisible = ref(false)

    const onScan = e => {
      const { target } = e

      if (target.classList.contains('btn-success')) {
        return
      }

      videoVisible.value = true

      codeReader
        .decodeOnceFromVideoDevice(undefined, 'video')
        .then(result => {

          barcodeResult.value = result.text

					target.classList.remove('btn-info')
					target.classList.add('btn-success')
					target.disabled = true
        })
        .catch(err => {
          console.error(err)
        });
    }

    return {
      barcodeResult,
      onScan,
      videoVisible,
    };
  },
});
</script>

<style scoped>
* {
  box-sizing: border-box;
}

.scan {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.scan__btn,
.scan__results {
  margin-top: 20px;
}

.scan__video-outer {
  position: relative;
  width: 100%;
  margin-top: 10px;
  border: 1px solid #ccc;
}

.scan__video-outer::before {
  content: '';
  display: block;
  width: 100%;
  padding-top: 75%;
}

.scan__video {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.scan__results {
  width: 100%;
	padding: 20px;
	border: 1px solid #ccc;
}

.scan__results__result {
	margin-top: 10px;
	font-size: 28px;
	line-height: 1em;
}
</style>
