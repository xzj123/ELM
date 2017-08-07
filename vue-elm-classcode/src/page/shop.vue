<template lang="html">
    <div class="shop-wrapper">
        <div class="shop-header">
            <div class="background">
                <img :src="seller.avatar" alt="">
            </div>
            <div class="shop-header-back">
                <router-link tag="i" to="/index" class="icon iconfont icon-back"></router-link>
            </div>
            <div class="seller-info">
                <img :src="seller.avatar" alt="">
                <div class="detail">
                    <div class="title">
                        {{seller.name}}
                    </div>
                    <div class="dilivery">
                        商家配送： {{seller.deliveryTime}} 分钟送达 / 配送费 ￥ {{seller.deliveryPrice}}
                    </div>
                    <div class="notice">
                        本公司提供专业发票
                    </div>
                </div>
            </div>
        </div>
        <div class="tab-box">
            <div class="item act">
                <span>商家</span>
            </div>
            <div class="item">
                <span>评论</span>
            </div>
        </div>

        <menuview></menuview>
    </div>
</template>

<script>
import menuview from '@/components/menuview'
export default {
	components: {
		menuview
	},
	data() {
		return {
			seller: {
				type: Array
			}
		}
	},
	mounted() {
		let idx = this.$route.params.idx;
		this.axios.get('http://localhost:8080/api/seller').then(res => {
			let sellers = res.data.data;
			this.seller = sellers[idx];
		})
	}
}
</script>

<style lang="less">
@import '../../static/less/var.less';
.shop-wrapper {
    .shop-header {
        height: @base*261rem;
        position: relative;
        background: rgba(7,17,27,0.5);
        .background {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            filter:blur(20px);
            img {
                width: 100%;
                height: 100%;
            }
        }
        .shop-header-back {
            height: @base*55rem;
            padding: @base*8rem @base*20rem;
            i {
                color: #fff;
                padding: @base*5rem;
                font-size: @base*40rem;
                font-weight: 700;
            }
        }
    }
    .seller-info {
        height: @base*130rem;
        padding: 0 @base*20rem;
        display: flex;
        justify-content: space-between;
        img {
            width: @base*128rem;
            height: @base*128rem;
            border-radius: 5px;
            border: 1px solid #fff;
        }
        .detail {
            margin-left: @base*20rem;
            flex: 1;
            display: flex;
            flex-direction: column;
            color: #fff;
            font-size: @base*20rem;
            .title {
                font-size: @base*30rem;
            }
        }
    }
    .tab-box {
        height: @base*90rem;
        background: #fff;
        display: flex;
        line-height: @base*90rem;
        .item {
            width: 50%;
            text-align: center;
            span {
                padding-bottom: @base*20rem;
            }
            &.act {
                span {
                    border-bottom: @base*3rem solid @mc;
                }
            }
        }
    }
}
</style>
