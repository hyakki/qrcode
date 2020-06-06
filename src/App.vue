<template>
  <div id="app">
    <div class="app-inner">
      <Scan />
      <hr />
      <Deposit />
      <hr />
      <Generator />
      <hr />
      <Email />
    </div>
  </div>
</template>

<script>
import Scan from './components/Scan.vue'
import Generator from './components/Generator.vue'
import Deposit from './components/Deposit.vue'
import Email from './components/Email.vue'

export default {
  name: 'App',
  components: {
		Deposit,
    Generator,
    Scan,
		Email,
  }
}
</script>

<style>
	#app {
		box-sizing: border-box;
    background-color: #eee;
	}

  .app-inner {
    padding: 20px;
    max-width: 640px; 
    min-height: 100vh;
    margin: 0 auto;
    background-color: #fff;
  }

  h1 {
    margin: 0;
    color: #aaa;
  }

	hr {
		margin-top: 50px;
		margin-bottom: 0;	
	}

	.btn {
		min-width: 180px;
		text-align: left;
	}
</style>
