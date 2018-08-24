<template>
	<div class="goods">
		<div class="menu-wrapper" :goods="goods">{{goods}}</div>
		<div class="foods-wrapper"></div>
	</div>
</template>

<script>
	const ERROR_OK = 0;

	export default {
		props: {
			seller: {

			}
		},
		data() {
			return {
				goods: []
			}
		},
		created() {
			this.$http.get('/api/goods').then((response) => {
				response = response.body;
				if(response.errno == ERROR_OK) {
					this.goods = response.data;
					console.log("goods:" + this.goods)
				}
			})
		}
	}
</script>

<style scoped>
	.goods {
		position: absolute;
		display: flex;
		width: 100%;
		top: 174px;
		bottom: 46px;
		overflow: hidden;
	}
	
	.goods>.menu-wrapper {
		flex: 0 0 80px;
		width: 80px;
		background: #f3f5f7;
	}
	
	.goods>.foods-wrapper {
		flex: 1;
		background: black;
	}
</style>