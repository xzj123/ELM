<template lang="html">
    <div class="footer-wrapper">
        <router-link to="/index" active-class="act" tag="div" class="item">
            <i class="icon iconfont icon-changyonglogo40"></i>
            <span>外卖</span>
        </router-link>

        <router-link  active-class="act"  to="/discovery" tag="div" class="item">
            <i class="icon iconfont icon-faxian"></i>
            <span>发现</span>
        </router-link>
        <router-link active-class="act"  to="/order" tag="div" class="item">
            <i class="icon iconfont icon-icon"></i>
            <span>订单</span>
        </router-link>
        <router-link active-class="act"  to="/mine" tag="div" class="item">
            <i class="icon iconfont icon-wode"></i>
            <span>我的</span>
        </router-link>
    </div>
</template>

<script>
export default {}
</script>

<style lang="less">
@import '../../static/less/var.less';

.footer-wrapper {
    position: fixed;
    bottom: 0;
    left: 0;
    width: 100%;
    height: @base*100rem;
    display: flex;
    background: #fff;
    border-top: 1px solid @bc1;
    .item {
        display: flex;
        flex-direction: column;
        width: 25%;
        height: 100%;
        text-align: center;
        padding-top: @base*20rem;
        color: @fc1;
        i {
            font-size: @base*32rem;
        }
        span {
            font-size: @base*24rem;

        }
        &.act {
            color: @mc;
        }
    }
}
</style>
