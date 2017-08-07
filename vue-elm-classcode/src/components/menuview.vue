<template lang="html">
    <div class="">
        <div class="menuview-wrapper">
            <div class="menu-wrapper" ref="menuScroll">
                <div class="">
                    <div @click="scrollTo(index)" class="item" :class="{act:menuIndex===index}" v-for="(m,index) in goods">
                        {{m.name}}
                    </div>
                </div>
            </div>
            <div class="food-wrapper" ref="foodScroll">
                <div class="">
                    <dl class="" v-for="foods in goods">
                        <dt>{{foods.name}}</dt>
                        <dd v-for="food in foods.foods">
                            <img :src="food.icon" alt="">
                            <div class="food-info">
                                <h4>{{food.name}}</h4>
                                <p class="desc" v-if="food.info">{{food.info.substr(0,20)+'...'}}</p>
                                <p class="rating">
                                    月售{{food.sellCount}}份 好评率{{food.rating}}%
                                </p>
                                <p class="price">
                                    <span>￥{{food.price}}</span>
                                    <cartbutton @ball-click="ballEvent" :food="food"></cartbutton>
                                </p>
                            </div>
                        </dd>
                    </dl>
                </div>
            </div>
        </div>
        <shopcart ref="shopCart" :selectedFoods="selectedFoods"></shopcart>
    </div>

</template>

<script>
import shopcart from '@/components/shopcart';
import BScroll from 'better-scroll';
import cartbutton from '@/components/cartbutton';
export default {
	data() {
		return {
			scrollY: 0,
			heights: [],
			goods: {
				type: Array
			}
		}
	},
	components: {
		shopcart,
		BScroll,
		cartbutton
	},
	mounted() {
		this.axios.get('http://localhost:8080/api/goods').then(res => {

			if (res.data.errno == 0) {
				this.goods = res.data.data;
				//数据渲染完成之后再执行
				this.$nextTick(() => {
					this._initScroll();
					this._countHeight();
				})
			}
		})

	},
	computed: {
		menuIndex() {
			for (let i = 0; i < this.heights.length; i++) {
				let now = this.heights[i];
				let next = this.heights[i + 1];
				if (now >= this.scrollY && this.scrollY < next) {

				} else if (!next) {
					return i;
				}
			}
			return 0;
		},
		selectedFoods() {
			let foods = [];
			if (this.goods.length == 0) {
				return []
			};
			for (let i = 0; i < this.goods.length; i++) {
				let good = this.goods[i];
				good.foods.forEach(food => {
					if (food.count > 0) {
						foods.push(food);
					}
				})
			}
			return foods;
		}
	},
	methods: {
		ballEvent(target) {
			this.$refs.shopCart.ballClick(target);
			//小球事件

		},
		//初始化滚动条
		_initScroll() {
			this.menuScroll = new BScroll(this.$refs.menuScroll, {
				click: true
			});
			this.foodScroll = new BScroll(this.$refs.foodScroll, {
				probeType: 3,
				click: true
			});
			this.foodScroll.on('scroll', (pos) => {
				this.scrollY = Math.abs(Math.round(pos.y));
			})
		},
		_countHeight() {
			let dts = this.$refs.foodScroll.getElementsByTagName('dt');
			for (let i = 0; i < dts.length; i++) {
				this.heights.push(dts[i].offsetTop);
			}
		},
		scrollTo(index) {
			this.menuIndex = index;
			this.foodScroll.scrollTo(0, -this.heights[index], 100);
		}
	}
}
</script>

<style lang="less" scoped>
@import '../../static/less/var.less';
.menuview-wrapper {
    position: absolute;
    top: @base*351rem;
    bottom: @base*96rem;
    width: 100%;
    display: flex;

    overflow: hidden;

    .menu-wrapper {
        width: @base*180rem;
        height: 100%;
        background: #fff;
        .item {
            padding: @base*40rem @base*20rem;
            border-bottom: 1px solid @bc1;
            color: @fc1;
            &.act {
                border-left: @base*5rem solid @mc;
            }
        }
    }
    .food-wrapper {
        background: #fff;
        flex: 1;
        dl {
            dt {
                color: #333;
                font-size: @base*24rem;
                padding: @base*8rem @base*20rem;
                background: #f5f5f5;
            }
            dd {
                padding: @base*10rem @base*20rem;
                display: flex;
                border-bottom: 1px solid @bc1;
                img {
                    width: @base*104rem;
                    height: @base*104rem;
                }
                .food-info {
                    flex: 1;
                    display: flex;
                    flex-direction: column;
                    margin-left: @base*20rem;
                    color: @fc1;
                    h4 {
                        font-size: @base*30rem;
                        color: #333;
                    }
                    .desc {
                        font-size: @base*8rem;
                        color: @fc1;
                    }
                    .rating {
                        font-size: @base*8rem;
                    }
                    .price {
                        color: @mc;
                        font-size: @base*30rem;
                        font-weight: 700;
                        display: flex;
                        justify-content: space-between;
                    }
                }
            }
        }
    }
}
</style>
