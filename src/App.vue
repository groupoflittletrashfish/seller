<template>
	<div id="app">
		<div class="header">
			<v-header :seller="seller"></v-header>
		</div>
		<div class="tab">
			<div class="tab-item">
				<router-link to="{
					path:'goods',
					params:{
						seller:seller
					}
				}">商品</router-link>
			</div>
			<div class="tab-item">
				<router-link to="/ratings">评论</router-link>
			</div>
			<div class="tab-item">
				<router-link to="/seller">商家</router-link>
			</div>
		</div>
		<router-view :seller="seller"></router-view>
	</div>
</template>

<script type="text/ecmascript-6">
	import header from './components/header/header'
	export default {
		data() {
			return {
				seller: {}
			}
		},
		created() {
			this.$http.get('/api/seller').then(
				(response) => {
					response = response.body;
					console.log(response)
					if(response.errno == 0) {
						this.seller = response.data
					}
				}
			)
		},
		components: {
			'v-header': header
		}
	}
</script>

<style scoped>
	#app>.tab {
		display: flex;
		width: 100%;
		height: 40px;
		line-height: 40px;
		position: relative;
	}
	
	@media screen {
		#app>.tab:after {
			display: block;
			position: absolute;
			left: 0;
			bottom: 0;
			border-top: 1px solid black;
			content: ' ';
			width: 100%;
			transform: scaleY(0.1);
		}
	}
	
	.tab-item {
		flex: 1;
		text-align: center;
	}
	
	.tab-item>a {
		color: rgb(77, 85, 93);
		display: block;
		font-size: 14px;
		text-decoration: none;
	}
	
	.tab-item>.active {
		color: rgb(240, 20, 20)
	}
</style>