<template>
  <div class="generator">
    <h1>3.Generator</h1>

    <button class="generator__retrieve btn btn-info"
						@click.prevent="retrieve($event)">
			Retrieve Data	
    </button>

    <div v-if="barcode">
      <div class="input-group">
        <div class="input-group-prepend">
          <span class="input-group-text">
            name
          </span>
        </div>
        <input class="form-control" v-model="name" type="text" />
      </div>

      <div class="input-group">
        <div class="input-group-prepend">
          <span class="input-group-text">
            address
          </span>
        </div>
        <input class="form-control" v-model="address" type="text" />
      </div>

      <div class="input-group">
        <div class="input-group-prepend">
          <span class="input-group-text">
            barcode
          </span>
        </div>
        <input class="form-control" v-model="barcode" type="text" />
      </div>

      <div class="input-group">
        <div class="input-group-prepend">
          <span class="input-group-text">
            token
          </span>
        </div>
        <input class="form-control" v-model="token" type="text" />
      </div>
    </div>

    <button class="generator__generate-pdf btn btn-info"
						@click.prevent="generatePDF($event)">
			Generate PDF 
    </button>

		<div class="generator__pdf" v-if="pdfURL">
			Le pdf a ete genere a l'adresse suivante: <br />
			{{ pdfURL }}
		</div>

    <button class="generator__generate-qr btn btn-info"
						@click.prevent="generateQR($event)">
			Generate QRCode 
    </button>

    <div class="canvas-container" :class="{'is-visible': displayQR}">
      <canvas ref="canvas"></canvas>
    </div>
  </div>
</template>

<script>
import {
  defineComponent,
  ref,
} from "@vue/composition-api";
import QRCode from "qrcode";

export default defineComponent({
  name: "generator",
  setup() {
    const canvas = ref()
    const name = ref()
    const address = ref()
    const barcode = ref()
    const token = ref()
		const pdfURL = ref()
    const displayQR = ref(false)

    const createQR = (str) => {
      QRCode.toCanvas(
        canvas.value,
        str,
        {
          width: 400,
        },
        (err) => {
          err && console.error(err)
        }
      )
    }

    const generatePDF = e => {
			const { target } = e

			if (target.classList.contains('btn-success')) {
				return
			}

			const { origin } = window.location

			pdfURL.value = `${origin}/${token.value}/recu-05-06-2020-18-07.pdf`

			target.classList.remove('btn-info')
			target.classList.add('btn-success')
			target.disabled = true
    }

    const retrieve = e => {
			const { target } = e

			if (target.classList.contains('btn-success')) {
				return
			}

      name.value = 'Maxime Parisse'
      address.value = '10 rue de Geneve, 1140 Evere, Belgique'
			barcode.value = document.querySelector('.scan__results__result').innerHTML
			token.value = 'un8ay9ol79ju5n6hh6p41c'

			target.classList.remove('btn-info')
			target.classList.add('btn-success')
			target.disabled = true
    }


    const generateQR = e => {
			const { target } = e

			if (target.classList.contains('btn-success')) {
				return
			}

      createQR(pdfURL.value)

      displayQR.value = true

			target.classList.remove('btn-info')
			target.classList.add('btn-success')
			target.disabled = true
    }

    return {
      address,
      barcode,
      canvas,
      displayQR,
      generateQR,
      generatePDF,
      name,
      token,
			retrieve,
			pdfURL,
    }
  },
})
</script>

<style scoped>
.generator {
	display: flex;
	flex-direction: column;
	align-items: flex-start;
	margin-top: 40px;
}

.generator__generate-qr,
.generator__generate-pdf {
	margin-top: 20px;
}

.input-group:nth-of-type(1) {
	margin-top: 20px;
}

.input-group + .input-group {
	margin-top: 5px;
}

span.input-group-text {
	min-width: 90px;
}

.btn:nth-of-type(1) {
	margin-top: 20px;
}

.generator__pdf {
  width: 100%;
  overflow: hidden;
	margin-top: 20px;
	border: 1px solid #ccc;
	padding: 20px;
}

.canvas-container {
  position: relative;
  display: none;
  width: 100%;
  border: 1px solid #ccc;
  margin-top: 20px;
}

.canvas-container.is-visible {
  display: block;
}

.canvas-container::before {
  content: '';
  display: block;
  width: 100%;
  padding-top: 100%;
}

canvas {
  position: absolute;
  top: 0;
  left: 0;
  width: 100% !important;
  height: 100% !important;
  max-width: 100%;
  max-height: 100%;
}
</style>
