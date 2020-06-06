<template>
  <div class="deposit">
    <h1>2.Deposit</h1>
    <button class="deposit__open btn btn-info"
            @click.prevent="validate($event)">
      Open the box
    </button>
    <button class="deposit__deliver btn btn-info"
            @click.prevent="validate($event)">
      Deliver the package
    </button>
    <button class="deposit__close btn btn-info"
            @click.prevent="validate($event)">
      Close the box
    </button>
  </div>
</template>

<script>
import {
  defineComponent,
} from "@vue/composition-api";

export default defineComponent({
  name: 'deposit',
  setup() {
    const validate = e => {
      const { target } = e

      if (target.classList.contains('btn-success')) {
        return
      }

      target.classList.remove('btn-info')
      target.classList.add('btn-success')
      target.disabled = true
    }

    return {
      validate, 
    }
  },
});
</script>

<style scoped>
.deposit {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
	margin-top: 40px;
}

.btn:nth-of-type(1) {
  margin-top: 20px;
}

.btn + .btn {
  margin-top: 5px;
}
</style>
