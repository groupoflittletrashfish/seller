<template>
	<div class="goods">
		<div class="menu-wrapper" :goods="goods">
			<ul>
				<li v-for="item in goods" class="menu-item">
					<span class="text">
						<span v-show="item.type" class="icon" :class="classMap[item.type]">	
					</span>
					<span>{{item.name}}</span>
					</span>
				</li>
			</ul>
		</div>
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
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
			this.$http.get('/api/goods').then((response) => {
				response = response.body;
				if(response.errno == ERROR_OK) {
					this.goods = response.data;
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
	
	.menu-item {
		display: table;
		height: 54px;
		width: 56px;
		/*line-height: 14px;*/
		position: relative;
		padding: 0 12px;
	}
	
	.menu-item:after {
		display: block;
		position: absolute;
		left: 0;
		bottom: 0;
		border-top: 1px solid black;
		content: ' ';
		width: 100%;
		transform: scaleY(.3);
	}
	
	.menu-item .icon {
		vertical-align: top;
		width: 12px;
		height: 12px;
		background-size: 12px 12px;
		background-repeat: no-repeat;
	}
	
	.decrease {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('decrease_1@3x.png');
		background-size: 18px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.discount {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('discount_1@3x.png');
		background-size: 18px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.guarantee {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('guarantee_1@3x.png');
		background-size: 18px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.invoice {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('invoice_1@3x.png');
		background-size: 18px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.special {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('special_1@3x.png');
		background-size: 18px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.menu-item .text {
		display: table-cell;
		font-size: 12px;
		width: 56px;
		vertical-align: middle;
	}
</style>