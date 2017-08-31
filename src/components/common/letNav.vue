<template>
    <div class="leftNav">
        <ul>
            <li>
                <i class="icon iconfont icon-cartfill"></i>
                <div>收银</div>
            </li>
             <li>
                <i class="icon iconfont icon-shop"></i>
                <div>店铺</div>
            </li>
            <li>
                <i class="icon iconfont icon-shop"></i>
                <div>商品</div>
            </li>
            <li>
                <i class="icon iconfont icon-shop"></i>
                <div>会员</div>
            </li>
            <li>
                <i class="icon iconfont icon-shop"></i>
                <div>统计</div>
            </li>
            <li>
                <i class="icon iconfont icon-shop"></i>
                <div>设置</div>
            </li>
        </ul>
    </div>
</template>
<script >
    export default{
        data(){
            return {
                mes:"22"
            }
        }
    }
</script>
<style type="text/css" >
    .leftNav{
        color:#fff;
        font-size:10px;
        height: 100%;
        background-color:#1d8ce0;
        float:left;
        width:80px;
        text-align:center;
    }
    .leftNav>i{
        font-size:24px;
    }
    .leftNav>ul{
        padding:0px;
        margin:0px;
    }
    .leftNav>ul>li{
        list-style:none;
        text-align:center;
        border-bottom:1px solid #2a20ff;
        padding:10px;
    }
</style>