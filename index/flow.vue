<template>
	<view class="">
		<u-parse :content="message" ></u-parse>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				message:''
			};
		},
		methods:{
			getMessage () {
				this.$api.agreement({name:'delivery_process'}).then(res=>{
					if (res.code === 1) {
						this.message = res.data.content
					}
				})
			}
		},
		onLoad() {
			this.getMessage()
		}
	}
</script>

<style lang="scss">

</style>
