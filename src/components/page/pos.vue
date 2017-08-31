<template>
    <div class="pos">
        <el-row>
            <el-col :span='7' class="pos-order" id="order-list">
               <el-tabs>
                   <el-tab-pane label="点餐">
                       <el-table :data="tableData" border style="width:100%">
                           <el-table-column prop="goodsName" label="商品名称">
                           </el-table-column>
                            <el-table-column prop="count" width='70' label="数量">
                           </el-table-column>
                            <el-table-column prop="price" width='70' label="金额">
                           </el-table-column>
                            <el-table-column prop="doogName" width='100' fixed="right" label="操作">
                                <template scope="scope">
                                    <el-button type="text" @click="delSingleGoods(scope.row)" size="small">删除</el-button>
                                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                                </template>
                           </el-table-column>
                       </el-table>
                       <div class="totalDiv">
                           <small>数量：</small>{{totalCount}} &nbsp;&nbsp;&nbsp;&nbsp;<small>金额：</small>{{totalMoney}}元
                       </div>
                       <div class="div-btn">
                          <el-button type="warning">挂单</el-button>
                          <el-button type="danger" @click="delAllGoods">删除</el-button>
                          <el-button type="success" @click="checkedOut">结账</el-button>
                       </div>
                   </el-tab-pane>
                   <el-tab-pane label="挂单">
                        挂单
                   </el-tab-pane>
                   <el-tab-pane label="外卖">
                        外卖
                   </el-tab-pane>
               </el-tabs>

            </el-col>

            <el-col :span="17">
                <div class="ofter-goods">
                    <div class="title"> 常用商品</div>
                    <ul class="ofter-list">
                        <li v-for="item in oftenGoods" @click="addOrderList(item)">
                            <span>{{item.goodsName}}</span>
                            <span class="ofter-price">￥{{item.price}}元</span>
                        </li>
                    </ul>
                </div>
                <div class="goodType">
                    <el-tabs>
                        <el-tab-pane label="汉堡">

                            <ul class='cookList'>
                               <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>

                        </el-tab-pane>
                        <el-tab-pane label="小食">
                            <ul class='cookList'>
                               <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="饮料">
                            <ul class='cookList'>
                               <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>
                        <el-tab-pane label="套餐">
                            <ul class='cookList'>
                               <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                                    <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                                    <span class="foodName">{{goods.goodsName}}</span>
                                    <span class="foodPrice">￥{{goods.price}}元</span>
                                </li>
                            </ul>
                        </el-tab-pane>

                    </el-tabs>
                </div>

            </el-col>
        </el-row>
    </div>
</template>
<script >
import axios from 'axios'
export default{
    name:"pos",
    data(){
        return {
            tableData:[],
            oftenGoods:[],
            type0Goods:[],
            type1Goods:[],
            type2Goods:[],
            type3Goods:[],
            totalMoney:0,
            totalCount:0
        }
    },
    created(){

        axios.get('http://jspang.com/DemoApi/oftenGoods.php')
        .then(reponse=>{
            this.oftenGoods=reponse.data;
        })
        .catch(error=>{
           alert("网络错误")
        })

        axios.get("http://jspang.com/DemoApi/typeGoods.php")
        .then(reponse=>{
            console.log(reponse)
            this.type0Goods=reponse.data[0];
            this.type1Goods=reponse.data[1];
            this.type2Goods=reponse.data[2];
            this.type3Goods=reponse.data[3];
        })
        .catch(error=>{
            alert("网络异常")
        })


    },
    mounted(){
        var orderHeight=document.body.clientHeight
        var orderList=document.getElementById("order-list")

        orderList.style.height=orderHeight+"px"
    },
    methods:{
        addOrderList(goods){
            this.totalMoney=0;
            this.totalCount=0;
            //商品是否存在
            let isHave=false;
            console.log(goods)
            console.log(this.tableData)
            for(let i=0;i<this.tableData.length;i++){
                    console.log(this.tableData[i])
                    if(this.tableData[i].goodsId == goods.goodsId){
                        isHave=true
                    }
            }
            //根据判断的值编写业务逻辑
            if(isHave){
                //改变列表中商品的数量
                let arr=this.tableData.filter(a=>a.goodsId == goods.goodsId);
                arr[0].count++;

            }else {
                let newGoods={
                    goodsId:goods.goodsId,
                    goodsName:goods.goodsName,
                    price:goods.price,count:1
                };
                 this.tableData.push(newGoods);

            }
            this.getALLMoney()


        },
        //删除单个商品
        delSingleGoods(goods){
            this.tableData=this.tableData.filter(a=>a.goodsId !==goods.goodsId)
            this.getALLMoney()

        },
        //会总数量和金额
        getALLMoney(){
            this.totalCount=0;
            this.totalMoney=0
            if(this.tableData){
                this.tableData.forEach((ele)=>{
                this.totalMoney=this.totalMoney+(ele.price*ele.count);
                this.totalCount+=ele.count
            })
            }
        },
        //删除
        delAllGoods(){
            this.totalCount=0;
            this.totalMoney=0;
            this.tableData=[]
        },
        //结账
        checkedOut(){
            if(this.totalCount !==0){
                this.totalCount=0;
                this.totalMoney=0;
                this.tableData=[];
                this.$message({
                    message:"结账成功。",
                    type:"success"
                })
            }else {
                this.$message.error("空不能")
            }
        }
    }
}
</script>
<style type="text/css" scoped>
.pos-order{
    background:#f9fafc;
    border-right:1px solid #c0ccda;
    height:100%;
}
.div-btn{
    margin-top:10px;
}
.ofter-goods .title{
    height:20px;
    border-bottom:1px solid #d3dce6;
    background-color:#f9fafc;
    padding:10px;
    text-align: left;
}

.ofter-goods .ofter-list li{
    list-style:none;
    float:left;
    border:1px solid #e5e9f2;
    padding:10px;
    margin:10px;
    background-color:#fff;
    cursor:pointer;
}


.ofter-price{
    color: #5887ff;
}

.goodType{
    clear:both;
}

.cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auto;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
       cursor:pointer;

   }
   .cookList li span{
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;

   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }

   .totalDiv{
    background-color: #fff;
    padding:10px;
    border-bottom:1px solid red;
   }
</style>