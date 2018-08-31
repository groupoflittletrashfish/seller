<template>
	<div class="cartcontrol">
		<transition name="move">
			<div class="cart-decrease" v-show="food.count>0" @click="decreaseCart">
				<span class="inner fa fa-minus-circle"></span>
			</div>
		</transition>
		<div class="cart-count" v-show="food.count>0">{{food.count}}</div>
		<div class="cart-add fa fa-plus-circle" @click="addCart($event)"></div>
	</div>
</template>

<script>
	import Vue from 'vue'
	export default {
		props: {
			food: {
				type: Object
			}
		},
		created() {
			
		},
		methods: {
			addCart(e) {
				if(!this.food.count) {
					Vue.set(this.food, 'count', 1)
				} else {
					Vue.set(this.food, 'count', this.food.count + 1)
				}
//				this.$emit("addCart",e)
			},
			decreaseCart() {
				if(this.food.count === 0) {
					return;
				}
				Vue.set(this.food, 'count', this.food.count - 1)
			}
		}
	}
</script>

<style scoped>
	.cartcontrol {
		font-size: 0;
	}
	
	.cart-decrease {
		display: inline-block;
		padding: 6px;
		font-size: 24px;
		line-height: 24px;
		color: rgb(0, 160, 220);
	}
	
	.cart-count {
		display: inline-block;
		padding: 6px;
		font-size: 18px;
		color: rgb(0, 160, 220);
		vertical-align: top;
		line-height: 24px;
		text-align: center;
		font-size: 10px;
		color: rgb(147, 153, 159);
	}
	
	.cart-add {
		display: inline-block;
		padding: 6px;
		font-size: 24px;
		line-height: 24px;
		color: rgb(0, 160, 220);
	}
	
	.inner {
		display: inline-block;
		line-height: 24px;
		font-size: 24px;
		color: rgb(0,160,220);
	}
	
	.move-enter-active,
	.move-leave-active {
		transition: all .4s ease
	}
	
	.move-enter,
	.move-leave-to {
		opacity: 0;
		transform: translateX(24px);	
	}
</style>