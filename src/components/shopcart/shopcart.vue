<template>
	<div class="shopcart">
		<div class="content" @click="toggleList">
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{highlight:totalCount>0}">
						<i class="fa fa-shopping-cart" :class="{highlight:totalCount>0}"></i>
					</div>
					<div class="num" v-show="totalCount>0">{{totalCount}}</div>
				</div>
				<div class="price" :class="{highlight:totalCount>0}">{{totalPrice}}元</div>
				<div class="desc">另需配送费￥{{deliveryPrice}}元</div>
			</div>
			<div class="content-right">
				<div class="pay" :class="{enough:enough,notenough:notenough}">
					{{payDesc}}
				</div>
			</div>
		</div>
		<div class="ball-container">
			<div v-for="ball in balls">
				<transition name="drop">
					<div class="ball" v-show="ball.show">
						<div class="inner inner-hook"></div>
					</div>
				</transition>
			</div>

			<!--<transition-group name="drop" @before-enter="beforeEnter"  v-on:enter="enter"  v-on:after-enter="afterEnter">
				<div v-for="(ball,index) in balls" :key="index" v-show="ball.show" class="ball" transition="drop">
					<div class="inner inner-hook"></div>
				</div>
			</transition-group>-->
		</div>
		<transition name="fold">
			<div class="shopcart-list" v-show="listShow">
				<div class="list-header">
					<h1 class="title">购物车</h1>
					<span class="empty">清空</span>
				</div>
				<div class="list-content">
					<ul>
						<li v-for="food in selectFoods">
							<span class="name">{{food.name}}</span>
							<div class="price">
								<span>￥{{food.price*food.count}}</span>
							</div>
							<div class="cartcontrol-wapper">
								<cartcontrol :food="food"></cartcontrol>
							</div>
						</li>
					</ul>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
	import cartcontrol from '../cartcontrol/cartcontrol'
	export default {
		props: {
			deliveryPrice: {
				type: Number,
				default: 0
			},
			minPrice: {
				type: Number,
				default: 0
			},
			selectFoods: {
				type: Array,
				default() {
					return [];
				}
			}
		},
		components: {
			cartcontrol
		},
		data() {
			return {
				enough: false,
				notenough: false,
				balls: [{
						show: false
					},
					{
						show: false
					}, {
						show: false
					}, {
						show: false
					}, {
						show: false
					}
				],
				dropBall: [],
				fold: true
			}
		},
		methods: {
			drop(el) {
				for(let i = 0; i < this.balls.length; i++) {
					let ball = this.balls[i]
					if(!ball) {
						ball.show = true
						ball.el = el
						this.dropBall.push(ball)
						return;
					}
				}
			},
			beforeEnter(el) {
				let count = this.balls.length
				while(count--) {
					let ball = this.balls[count]
					if(ball.show) {
						let rect = ball.el.getBoundingClientRect();
						let x = rect.left - 32
						let y = -(window.innerHeight - rect.top - 22);
						el.style.display = ''
						el.style.webkitTransform = `translate3d(0,${y}px,0)`
						el.style.transform = `translate3d(0,${y}px,0)`
						let inner = el.target.getElementByClassName('inner-hook')[0]
						inner.style.webkitTransform = `translate3d(${x}px,0,0)`
						inner.style.transform = `translate3d(${x}px,0,0)`
					}
				}

			},
			enter(el) {
				let rf = el.offsetHeight;
				this.$nextTick(() => {
					el.style.webkitTransform = 'translate3d(0,0,0)'
					el.style.transform = 'translate3d(0,0,0)'
					let inner = el.target.getElementByClassName('inner-hook')[0]
					inner.style.webkitTransform = 'translate3d(0,0,0)'
					inner.style.transform = 'translate3d(0,0,0)'
				})
			},
			afterEnter(el) {
				let ball = this.dropBall.shift();
				if(ball) {
					ball.show = false;
					el.style.display = 'none';
				}
			},
			toggleList() {
				if(!this.totalCount) {
					return
				}
				this.fold = !this.fold
			}
		},
		computed: {
			totalPrice() {
				let total = 0;
				this.selectFoods.forEach((food) => {
					total += food.price * food.count;
				})
				return total;
			},
			totalCount() {
				let count = 0;
				this.selectFoods.forEach((food) => {
					count += food.count;
				})
				return count;
			},
			payDesc() {
				this.enough = false;
				this.notenough = true;
				if(this.totalPrice === 0) {
					return `￥${this.minPrice}元起送`;
				} else if(this.totalPrice < this.minPrice) {
					let diff = this.minPrice - this.totalPrice;
					return `还差￥${diff}元起送`;
				} else {
					this.enough = true;
					this.notenough = false;
					return "去結算";
				}
			},
			listShow() {
				if(!this.totalCount) {
					this.fold = true
					return false
				}
				let show = !this.fold;
				return show;
			}
		}
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
		font-size: 0;
	}
	
	.content-left {
		flex: 1;
	}
	
	.logo-wrapper {
		display: inline-block;
		position: relative;
		top: -10px;
		margin: 0 12px;
		padding: 6px;
		width: 56px;
		height: 56px;
		box-sizing: border-box;
		vertical-align: top;
		border-radius: 50%;
		background: #141d27;
	}
	
	.logo {
		width: 100%;
		height: 100%;
		border-radius: 50%;
		background: #2b343c;
		text-align: center;
	}
	
	.num {
		position: absolute;
		top: 0;
		right: 0;
		width: 24px;
		height: 16px;
		line-height: 16px;
		text-align: center;
		border-radius: 16px;
		font-size: 9px;
		color: white;
		font-weight: 700;
		background: rgb(240, 20, 20);
		box-shadow: 0 4px 8px 0 rgba(0, 0, 0, .4);
	}
	
	.fa-shopping-cart {
		line-height: 44px;
		font-size: 24px;
		color: #80858a;
		transform: scaleX(.9)scaleY(.9);
	}
	
	.price {
		display: inline-block;
		vertical-align: top;
		line-height: 24px;
		margin-top: 12px;
		box-sizing: border-box;
		padding-right: 12px;
		border-right: 1px solid rgba(255, 255, 255, .1);
		font-size: 16px;
		font-weight: 700;
		color: rgba(255, 255, 255, .4);
	}
	
	.desc {
		display: inline-block;
		vertical-align: top;
		line-height: 24px;
		margin: 12px 0 0 12px;
		font-size: 10px;
		color: rgba(255, 255, 255, .4);
	}
	
	.content-right {
		flex: 0 0 105px;
		background: green;
	}
	
	.content-right {
		flex: 0 0 105px;
		width: 105;
	}
	
	.content-right .pay {
		font-size: 12px;
		width: 100%;
		height: 48px;
		line-height: 48px;
		text-align: center;
		color: rgba(255, 255, 255, .4);
		font-weight: 700;
		background: #2b333b;
	}
	
	.logo.highlight {
		background: #66C0FF;
	}
	
	.fa.highlight {
		color: white
	}
	
	.price.highlight {
		color: white
	}
	
	.pay.notenough {
		background: #2b333b;
	}
	
	.pay.enough {
		background: #00b43c;
		color: #fff;
	}
	
	.ball-container .ball {
		position: fixed;
		left: 32px;
		bottom: 32px;
		z-index: 200;
		transition: all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41);
		/*贝塞尔曲线*/
	}
	
	.inner {
		width: 16px;
		height: 16px;
		border-radius: 50%;
		background: rgb(0, 160, 220);
	}
	
	.drop-enter,
	.drop-enter-active,
	.drop-leave-active {
		transition: all .4s cubic-bezier(0.49, -0.29, 0.75, 0.41)
	}
	
	.drop-enter,
	.drop-leave-to {
		opacity: 1;
		transform: translateX(24px);
	}
	
	.shopcart-list {
		position: absolute;
		top: 0;
		left: 0;
		z-index: -1;
		width: 100%;
		height: 100px
	}
	
	.fold-enter-active,
	.fold-leave-active {
		transition: all .5s
	}
	
	.fold-enter,
	.fold-leave-to {
		opacity: 1;
		transform: translate3d(0, -100%, 0);
	}
</style>