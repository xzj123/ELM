<template lang="html">

    <div class="index-wrapper" >

        <scroller ref="myScroller"  :on-infinite="infinite">

        <div class="index-header">
            <div class="position-bar">
                <div class="position-box">
                    <i class="icon iconfont icon-dingwei"></i>
                    <span class="text">东城区北京市政府</span>
                </div>
                <div class="weather-box">
                    ssdf
                </div>
            </div>
            <div class="search-box">
                搜索商家、商品
            </div>
            <div class="swipper-box">
                <div class="swiper-container" id="line-slide">
                    <div class="swiper-wrapper">
                        <div class="swiper-slide" style="width:auto">张亮麻辣烫</div>
                        <div style="width:auto" class="swiper-slide"> 烤肉饭</div>
                        <div style="width:auto" class="swiper-slide">水果捞</div>
                        <div class="swiper-slide" style="width:auto">肯德基</div>
                        <div style="width:auto" class="swiper-slide"> 珍珠奶茶</div>
                        <div style="width:auto" class="swiper-slide">千层蛋糕</div>
                        <div class="swiper-slide" style="width:auto">牛肉板面</div>
                        <div style="width:auto" class="swiper-slide"> 麻辣香锅</div>
                        <div style="width:auto" class="swiper-slide">我的菜</div>
                    </div>

                    <!-- 如果需要滚动条 -->
                    <div class="swiper-scrollbar"></div>
                </div>
            </div>

        </div>
        <div class="slide-box">
            <div class="swiper-container slide-menu">
                <div class="swiper-wrapper">
                    <div class="swiper-slide">Slide 1</div>
                    <div class="swiper-slide">Slide 2</div>
                    <div class="swiper-slide">Slide 3</div>
                    <div class="swiper-slide">Slide 4</div>
                    <div class="swiper-slide">Slide 5</div>
                    <div class="swiper-slide">Slide 6</div>
                    <div class="swiper-slide">Slide 7</div>
                    <div class="swiper-slide">Slide 8</div>
                    <div class="swiper-slide">Slide 9</div>
                    <div class="swiper-slide">Slide 10</div>
                    <div class="swiper-slide">Slide 11</div>
                    <div class="swiper-slide">Slide 12</div>
                    <div class="swiper-slide">Slide 13</div>
                    <div class="swiper-slide">Slide 14</div>
                    <div class="swiper-slide">Slide 15</div>
                </div>
                <!-- 如果需要分页器 -->
                <div class="swiper-pagination"></div>
            </div>
        </div>

        <div class="shop-box">
            <h3>推荐商家</h3>
            <ul>
                <router-link tag="li" :to="{path:'/shop/'+index}" v-for="(s,index) in sellers" :key="index">
                    <img :src="s.avatar" alt="">
                    <div class="info">
                        <div class="title">
                            {{s.name}}
                        </div>
                        <div class="star-box">
                            月售 {{s.sellCount}} 单
                        </div>
                        <div class="dilivery-box">
                            {{s.minPrice}} 元起送 / 配送费 {{s.deliveryPrice}} / ￥{{s.rankRate}}/人
                        </div>
                    </div>
                </router-link>
            </ul>
        </div>

        </scroller>
        <footerbar></footerbar>
    </div>
</template>

<script>
import Vue from 'vue'
import footerbar from '@/components/footerBar'
import VueAwesomeSwiper from 'vue-awesome-swiper'
import VueScroller from 'vue-scroller'
Vue.use(VueScroller)

export default {

	data() {
		return {
			// movieList: [],
			noData: false,
			sellers: [],
			totalSeller: []
		}
	},
	components: {
		footerbar,
		VueAwesomeSwiper,
	},
	created() {
		//获取seller所有数据
		this.axios.get('/api/seller').then(res => {
			if (res.data.errno == 0) {
				this.totalSeller = res.data.data;
				let temp = this.totalSeller.concat();
				this.sellers = temp.splice(0, 5);
			}
		})
	},
	mounted() {
		var mySwiper = new Swiper('#line-slide', {
			direction: 'horizontal',

			// 如果需要滚动条
			scrollbar: '.swiper-scrollbar',
			slidesPerView: 'auto', //'auto'
		})

		var menuSwiper = new Swiper('.slide-menu', {
			direction: 'horizontal',
			slidesPerColumn: 2,
			slidesPerView: 4,
			slidesPerGroup: 8,
			// 如果需要分页器
			pagination: '.swiper-pagination',

		})

		//获取seller所有数据
		this.axios.get('/api/seller').then(res => {
			if (res.data.errno == 0) {
				this.totalSeller = res.data.data;
				let temp = this.totalSeller.concat();
				this.sellers = temp.splice(0, 5);
			}
		})
	},
	methods: {
		infinite(done) {
			let self = this;
			if (this.noData) {
				setTimeout(() => {
					self.$refs.myScroller.finishInfinite(2);
				})
				return
			}


			let start = this.sellers.length;


			setTimeout(() => {
				let total = self.totalSeller.concat(0);
				let temp = total.splice(start, 5);
				if (temp.length == 0) {
					self.noData = true;
				}
				this.sellers = this.sellers.concat(temp);
				console.log(self.$refs.myScroller, 'myscroller');
				self.$refs.myScroller.resize();
				done();
			}, 1500)
		},
	}
}
</script>

<style lang="less">
@import '../../static/less/var.less';

.index-wrapper {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    bottom: @base*100rem;
    .index-header {
        height: @base*204rem;
        padding: @base*20rem @base*28rem;
        background-color: @mc;
        .position-bar {
            height: @base*69rem;
            display: flex;
            justify-content: space-between;
            line-height: @base*69rem;
            color: #fff;
            .position-box {
                width: @base*417rem;
            }
            .weather-box {
                width: @base*106rem;
            }
        }
        .search-box {
            height: @base*70rem;
            margin: @base*15rem;
            padding: @base*1rem;
            color: @fc1;
            text-align: center;
            background: #fff;
            border-radius: 20px;
            line-height: @base*70rem;
            font-size: @base*20rem;
        }
    }
    #line-slide .swiper-slide {
        padding: 0 @base*10rem;
        color: #fff;
        font-size: @base*18rem;
    }
    .slide-box {
        height: @base*354rem;
        .slide-menu {
            height: @base*354rem;
            .swiper-slide {
                height: 50%;
            }
        }
    }
    .shop-box {
        background: #fff;
        margin-top: @base*10rem;

        h3 {
            padding-left: @base*20rem;
            font-size: @base*30rem;
            padding: @base*20rem;
        }
        ul {
            width: 100%;
            list-style: none;
            li {
                display: flex;
                justify-content: space-between;
                padding: @base*10rem @base*20rem;
                border-bottom: 1px solid @bc1;
                img {
                    width: @base*90rem;
                    height: @base*90rem;
                }
                .info {
                    margin-left: @base*20rem;
                    display: flex;
                    flex: 1;
                    flex-direction: column;
                    font-size: @base*20rem;
                }
            }
        }
    }
}
</style>
