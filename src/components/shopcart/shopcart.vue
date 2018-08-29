<template>
	<div class="shopcart">
		<div class="content">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo">
						<span class="icon-shopping_cart"></span>
					</div>
				</div>
				<div class="price"></div>
				<div class="desc"></div>
			</div>
			<div class="content-right"></div>
		</div>
	</div>
</template>

<script>
	export default {

	}
</script>

<style scoped>
	.shopcart {
		position: fixed;
		left: 0;
		bottom: 0;
		z-index: 11;
		width: 100%;
		height: 48px;
	}
	
	.content {
		display: flex;
		background: #141d27;
	}
	
	.content-left {
		flex: 1;
	}
	
	.content-right {
		flex: 0 0 105px;
		width: 105px
	}
</style>