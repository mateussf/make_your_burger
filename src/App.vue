<template>
	<Navbar :logo="logo_src" :alt="alt"/>
	<router-view />
	<Footer />
</template>


<script>
import Navbar from './components/Navbar.vue'
import Footer from './components/Footer.vue'


export default {
	name: 'App',
	components: {
		Navbar,
		Footer
	},
	data(){
		return{
			logo_src: require('../public/img/logo.png'),
			alt: 'Logo'
		}
	}
}

</script>

<style>
	* {
		font-family: Helvetica, sans-serif;
		padding: 0;
		margin: 0;
		box-sizing: border-box;
	}

	.main-container {
		margin: 50px;
		min-height: 800px;
	}

	h1 {
		text-align: center;
		font-size: 42;
		margin-bottom: 30px;
		color: #222;
	}
</style>
