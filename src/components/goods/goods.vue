<template>
	<div class="goods">
		<div class="menu-wrapper" :goods="goods" ref="menu-wapper">
			<ul>
				<li v-for="(item,index) in goods" class="menu-item" :class="{'current':currentIndex===index}" @click="selectMenu(index,$event)">
					<span class="text">
						<span v-show="item.type" class="icon" :class="classMap[item.type]">	
					</span>
					<span>{{item.name}}</span>
					</span>
				</li>
			</ul>
		</div>
		<div class="foods-wrapper" ref="foodsWrapper">
			<ul ref="foodWrapper">
				<li v-for="item in goods" class="food-list food-list-hook">
					<h1 class="title">{{item.name}}</h1>
					<ul>
						<li v-for="food in item.foods" class="food-item">
							<div class="icon">
								<img :src="food.icon" width="57" height="57">
							</div>
							<div class="content">
								<h2 class="name">{{food.name}}</h2>
								<p class="desc">{{food.description}}</p>
								<div class="extra">
									<span>月售{{food.sellCount}}份</span>
									<span>好评率{{food.rating}}%</span>
								</div>
								<div class="price">
									<span class="now">￥{{food.price}}</span>
									<span v-show="food.oldPrice" class="old">￥{{food.oldPrice}}</span>
								</div>
							</div>
						</li>
					</ul>
				</li>
			</ul>
		</div>
		<!--购物车-->
		<shopcart>
			
		</shopcart>
	</div>
</template>

<script>
	import shopcart from '../shopcart/shopcart'
	const ERROR_OK = 0;

	export default {
		props: {
			seller: {

			}
		},
		components:{
			shopcart
		},
		data() {
			return {
				goods: [],
				listHeight: [],
				y: 0,
			}
		},
		computed: {
			currentIndex() {
				for(var i = 0; i < this.listHeight.length; i++) {
					var height1 = this.listHeight[i]
					var height2 = this.listHeight[i + 1]
					if(!height2 || (this.y >= height1 && this.y < height2)) {
						return i;
					}
				}
				return 0;
			}
		},
		created() {
			this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
			this.$http.get('/api/goods').then((response) => {
				response = response.body;
				if(response.errno == ERROR_OK) {
					this.goods = response.data;
				}
				this.$nextTick(() => {
					this.calHeight();
					this.initScore()
				})
			})

		},
		methods: {
			calHeight() {
				var foodList = $('.food-list-hook')
				let height = 0;
				this.listHeight.push(height);
				for(var i = 0; i < foodList.length; i++) {
					let item = foodList[i];
					height += item.clientHeight;
					this.listHeight.push(height);
				}
			},
			initScore() {
				var that = this;
				this.$refs.foodWrapper.onscroll = function() {
					that.y = Math.round(this.scrollTop)
				}
			},
			selectMenu(index,e){
				console.log(e.target)
				var top=this.listHeight[index];
				this.$refs.foodWrapper.scrollTop=top
			}
		}
	}
</script>

<style scoped>
	.goods {
		position: absolute;
		display: flex;
		width: 100%;
		top: 178px;
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
		border-top: 1px solid rgba(7, 17, 27, .1);
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
	
	.foods-wrapper {
		flex: 1
	}
	
	.foods-wrapper .title {
		padding-left: 14px;
		height: 26px;
		line-height: 26px;
		border-left: 2px solid #d9dde1;
		font-size: 12px;
		color: rgb(147, 153, 159);
		background: #f3f5f7;
	}
	
	.foods-wrapper .food-item {
		display: flex;
		margin: 18px 18px 0px 18px;
		position: relative;
		padding-bottom: 18px;
	}
	
	.foods-wrapper .food-item:after {
		width: 100%;
		content: ' ';
		position: absolute;
		bottom: 0;
		left: 0;
		border-bottom: 1px solid rgba(7, 17, 27, .1);
		transform: scaleY(.5)
	}
	
	.foods-wrapper .food-item:last-of-type:after {
		display: none;
		margin-bottom: 0;
	}
	
	.foods-wrapper>ul,
	.menu-wrapper ul {
		overflow: auto;
		height: 100%;
	}
	
	.foods-wrapper .icon {
		flex: 0 0 57px;
		margin-right: 10px;
	}
	
	.foods-wrapper .content {
		flex: 1;
	}
	
	.foods-wrapper .content .name {
		font-size: 14px;
		margin: 2px 0 8px 0;
		height: 14px;
		line-height: 14px;
		color: rgb(7, 17, 27);
	}
	
	.foods-wrapper .content .desc {
		margin-bottom: 8px;
		line-height: 12px;
		font-size: 10px;
		color: rgb(147, 153, 159);
	}
	
	.foods-wrapper .content .extra {
		line-height: 10px;
		color: rgb(147, 153, 159);
		font-size: 10px;
	}
	
	.foods-wrapper .content .price {
		font-weight: 700;
		line-height: 24px;
	}
	
	.price>.now {
		margin-right: 14px;
		font-size: 14px;
		color: rgb(240, 20, 20);
	}
	
	.price>.old {
		text-decoration: line-through;
		font-size: 10px;
		color: rgb(147, 153, 159);
	}
	
	.current {
		position: relative;
		margin-top: -1px;
		z-index: 1;
		background: white;
		font-weight: 700;
	}

</style>