<template lang="html">
    <div class="">
        什么啊
        <img v-for="p in pic" :src="p|imgPath" alt="">
        <footerbar></footerbar>
    </div>
</template>

<script>
import footerbar from '@/components/footerBar'
import Fiter from '@/filter/filter';
export default {
	data() {
		return {
			pic: [
				'../../static/image/1.png',
				'../../static/image/1.png',
				'../../static/image/1.png'
			]
		}
	},
	components: {
		footerbar
	},
	mounted() {}
}
</script>

<style lang="css">
</style>
