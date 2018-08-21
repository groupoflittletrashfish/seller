<template>
	<div class="header">
		<div class="content-wrapper">
			<!--头像-->
			<div class="avatar">
				<img width="64" height="64" :src="seller.avatar" />
			</div>
			<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				</div>
				<div class="description">
					{{seller.description}}/{{seller.deliveryTime}}分钟送达
				</div>
				<div class="support" v-if="seller.supports">
					<span :class="classMap[seller.supports[0].type]"></span>
					<span class="text">{{seller.supports[0].description}}</span>
				</div>
			</div>
		</div>
		<div class="bulletin-wrapper"></div>
	</div>
</template>

<script>
	export default {
		props: {
			seller: {}
		},
		created() {
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee']
		}
	}
</script>

<style>
	.header {
		color: white;
		background: black;
	}
	
	.content-wrapper {
		padding: 24px 12px 18px 24px;
		font-size: 0px;
	}
	
	.avatar {
		display: inline-block;
		font-size: 14px;
		vertical-align: top
	}
	
	.avatar>img {
		border-radius: 2px;
	}
	
	.content {
		margin-left: 16px;
		display: inline-block;
		font-size: 14px;
	}
	
	.title {
		margin: 2px 0 8px 0;
	}
	
	.brand {
		width: 30px;
		height: 18px;
		display: inline-block;
		background-image: url('brand@2x.png');
		background-size: 30px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.name {
		margin-left: 16px;
		font-size: 16px;
		line-height: 18px;
		font-weight: bold;
	}
	
	.description {
		margin-bottom: 10px;
		line-height: 12px;
		font-size: 12px;
	}
	
	.support .icon {
		display: inline-block;
		width: 12px;
		height: 12px;
		margin-right: 4px;
		background-size: 12px;
		background-repeat: no-repeat;
	}
	
	.decrease {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('decrease_1@2x.png');
		background-size: 18px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.discount {
		width: 30px;
		height: 18px;
		display: inline-block;
		background-image: url('discount_1@2x.png');
		background-size: 30px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.guarantee {
		width: 30px;
		height: 18px;
		display: inline-block;
		background-image: url('guarantee_1@2x.png');
		background-size: 30px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.invoice {
		width: 30px;
		height: 18px;
		display: inline-block;
		background-image: url('invoice_1@2x.png');
		background-size: 30px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.special {
		width: 18px;
		height: 18px;
		display: inline-block;
		background-image: url('special_1@2x.png');
		background-size: 30px 18px;
		background-repeat: no-repeat;
		vertical-align: text-top
	}
	
	.support span:first-of-type {
		vertical-align: top;
		display: inline-block;
	}
	
	.text {
		
	}
</style>