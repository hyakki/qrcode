<template>
  <div class="email">
    <h1>4. Email</h1>
    <button class="deposit__open btn btn-info"
            @click.prevent="validate($event)">
      Send email 
    </button>
  </div>
</template>

<script>
import {
  defineComponent,
} from "@vue/composition-api";

export default defineComponent({
  name: 'email',
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
.email {
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
