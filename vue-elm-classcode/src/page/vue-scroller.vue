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
            <ul style="width:100%;">
                <li v-for="m in movieList" style="width:100%;height:50px;">
                    <!-- <img :src="m.images.small" style="width:50px;height:50px;float:left;" alt="">
                    <h4 style="float:left">{{m.title}}</h4> -->
                    {{m}}
                </li>
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
			noData: '',
			movieList: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20]
		}
	},
	components: {
		footerbar,
		VueAwesomeSwiper,
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

		//获取豆瓣数据初始化
		// let url = '/api/v2/movie/in_theaters';
		// this.axios.get(url).then((res) => {
		// 	this.movieList = res.data.subjects;
		// })

	},
	methods: {
		infinite(done) {
			if (this.noData) {
				setTimeout(() => {
					this.$refs.myScroller.finishInfinite(2);
				})
				return;
			}
			let start = this.movieList.length;
			let self = this;


			setTimeout(() => {
				for (let i = start; i < start + 10; i++) {
					self.movieList.push(i);
				}
				console.log(start);

				if (start >= 30) {
					self.noData = '没有更多数据了';
				}
				self.$refs.myScroller.resize()
				done();
			}, 1500)

			//重新获取高度
			// console.log(this.$refs.myScroller);

			// if (this.noData) {
			// 	return;
			// }
			// let start = this.movieList.length;
			// let url = `/api/v2/movie/in_theaters?start=${start}&count=10`;
			// this.axios.get(url).then((res) => {
			// 	let temp = res.data.subjects;
			// if (temp.length == 0) {
			// 	this.noData = '没有数据了';
			// }
			// 	this.movieList = this.movieList.concat(temp);
			// 	done();
			// })
		},
		// refresh() {
		// 	console.log('refresh');
		// }
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
        background: red;
        .slide-menu {
            height: @base*354rem;
            .swiper-slide {
                height: 50%;
            }
        }
    }
}
</style>
