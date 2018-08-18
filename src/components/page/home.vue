<template>
    <div id="home">
        <el-row>
            <el-col :span="6" class="order" id="orderH">
                <el-tabs v-model="active" type="card" stripe="true" size="mini">
                    <el-tab-pane label="结账" name="first">
                        <el-table :data="orderData" :header-cell-style="fontleft">
                        <el-table-column prop="goodsName" label="商品名称" width=150></el-table-column>
                        <el-table-column  prop="name" label="数量" width=50></el-table-column>
                        <el-table-column prop="price" label="价格" ></el-table-column>
                        <el-table-column label="操作" width=150>
                            <template slot-scope="scope">
                            <div>
                                <el-button size="mini" >编辑</el-button>
                                <el-button size="mini" type="danger"  @click="orderDel(scope.row)">删除</el-button>
                            </div>
                            </template> 
                        </el-table-column>
                    </el-table> 
                    <p class="allPrice">总金额：<b>{{totalMoney}}</b>元</p>
                    <div class="order-btn">
                        <el-button type="success"> 结账</el-button>
                        <el-button type="warning">挂单</el-button>
                        <el-button type="danger" @click="delAll">取消订单</el-button>
                    </div>
                    </el-tab-pane>
                    <el-tab-pane label="挂单" name="second">配置管理</el-tab-pane>
                    <el-tab-pane label="外卖" name="third">角色管理</el-tab-pane>
                </el-tabs>
            </el-col>
            <el-col :span="18">
                <div class="tagProject">
                    <dl  v-for="item in tagProject" :key="item.name"  v-on:click="orderAdd(item)">
                        <dt>{{item.goodsName}}</dt>
                        <dd>&yen;{{item.price}}/个</dd>
                    </dl>
                    
                </div>
                <div class="productClassify">
                    <template>
                        <el-tabs v-model="classifyActive" type="card" @tab-click="getClassifyData" v-loading="loadingStatus">
                            <el-tab-pane label="主食" name="first" v-model="classifyID">
                               <div class="Classifyproduct">
                                    <figure v-for="(item,index) in classifyData" :key="index">
                                        <img :src="item.goodsImg" alt="">
                                        <dl>
                                            <dt>{{item.goodsName}}</dt>
                                            <dd>&yen;{{item.price}}元/个</dd>
                                            <dd><el-button type="success" icon="el-icon-check" size="mini" v-on:click="orderAdd(item)"></el-button></dd>
                                        </dl>
                                    </figure>
                               </div>
                            </el-tab-pane>
                            <el-tab-pane label="小食品" name="second" v-model="classifyID">
                                <div class="Classifyproduct">
                                    <figure v-for="(item,index) in classifyData" :key="index">
                                        <img :src="item.goodsImg" alt="">
                                        <dl>
                                            <dt>{{item.goodsName}}</dt>
                                            <dd>&yen;{{item.price}}元/个</dd>
                                            <dd><el-button type="success" icon="el-icon-check" size="mini" v-on:click="orderAdd(item)"></el-button></dd>
                                        </dl>
                                    </figure>
                               </div>
                            </el-tab-pane>
                            <el-tab-pane label="饮料" name="third" v-model="classifyID">
                                 <div class="Classifyproduct">
                                    <figure v-for="(item,index) in classifyData" :key="index">
                                        <img :src="item.goodsImg" alt="">
                                        <dl>
                                            <dt>{{item.goodsName}}</dt>
                                            <dd>&yen;{{item.price}}元/个</dd>
                                            <dd><el-button type="success" icon="el-icon-check" size="mini" v-on:click="orderAdd(item)"></el-button></dd>
                                        </dl>
                                    </figure>
                               </div>
                            </el-tab-pane>
                            <el-tab-pane label="套餐" name="fourth" v-model="classifyID">
                                 <div class="Classifyproduct">
                                    <figure v-for="(item,index) in classifyData" :key="index">
                                        <img :src="item.goodsImg" alt="">
                                        <dl>
                                            <dt>{{item.goodsName}}</dt>
                                            <dd>&yen;{{item.price}}元/个</dd>
                                            <dd><el-button type="success" icon="el-icon-check" size="mini" v-on:click="orderAdd(item)"></el-button></dd>
                                        </dl>
                                    </figure>
                               </div>
                            </el-tab-pane>
                        </el-tabs>
                    </template>
                </div>
            </el-col>
        </el-row>
    </div>
</template>
<script>
export default {
    name:'home',
    data(){
        return{
            loadingStatus:false,
            active: "first",
            classifyActive:'first',
            classifyID:0,
            totalMoney:'',
            orderData:[],
            tagProject: [],
            classifyData:[]

        }
    },
    methods:{
        delAll(){
            this.orderData=[];
            this.totalMoney=0;
        },
        orderAdd(e){
            for(let i=0;i<this.orderData.length;i++){
                if(e.goodsId==this.orderData[i].goodsId){
                    this.$message({
                        message: '警告哦，此商品已存在订单中',
                        type: 'warning',
                        duration:1500,
                    })
                    return;
                }
            }
                this.orderData.push(e);
                this.totalMoney=0;
                    if( this.orderData){
                        this.orderData.forEach((element)=>{
                        this.totalMoney= parseInt(this.totalMoney)+ parseInt(element.price);
                    })
                 };
             
        },
        orderDel(goods){
           
            this.orderData=this.orderData.filter(o=>o.goodsId != goods.goodsId)
            if(this.orderData.length==0){
                this.totalMoney=0;
            }
        },
        fontleft({row, column, rowIndex, columnIndex}){
           return 'text-align:center;'
        },
        getClassifyData(tab,event){
            this.loadingStatus=true;
            this.classifyID=tab.index;
            this.$axios.get('http://www.jspang.com/DemoApi/typeGoods.php')
            .then(e=>{
                this.classifyData=e.data[this.classifyID];
                setTimeout((e=>{ this.loadingStatus=false;}),800);
               
            })
            .catch(e=>{
                console.log(e);
            })
        }
       
    },
    mounted(){
        let menuH=document.body.clientHeight-60;
        document.getElementById('orderH').style.height=menuH+'px';

    },
    created(){
       
        this.$axios.get('http://www.jspang.com/DemoApi/oftenGoods.php')
        .then(e=>{
            this.tagProject=e.data;
        })
        .catch(e=>{
            console.log(e);
        });
        this.$axios.get('http://www.jspang.com/DemoApi/typeGoods.php')
        .then(e=>{
            this.classifyData=e.data[this.classifyID]
        })
        .catch(e=>{
            console.log(e);
        })

    }
}
</script>
<style>
*{
    padding:0;
    margin:0;
}
.order{
    background:#fff;
    border-right:2px solid #1D8ce0;
}
.order-btn{
    margin-top:30px;
}
.tagProject{
    width: 100%;
    height: 100%;
    padding:5px;
    background:#eee;
    overflow: hidden;
    cursor: pointer;
    display: flex;
    flex-flow: row wrap;
    justify-content: flex-start;
    box-sizing: border-box;
}
.tagProject dl{
    width:12%;
    height:50px;
    margin:5px 3px;
    font-size: 14px;
    display: flex;
    flex-flow: row wrap;
    justify-content: space-around;
    
    align-items: center;
    border:1px solid #ddd;
    border-radius: 5px;
    background: #fff;
    box-sizing: border-box;
}
.tagProject dl dd{
    color:#1D8ce0;
}
.Classifyproduct{
    display: flex;
    flex-flow: row wrap;
    justify-content: flex-start;
}
.Classifyproduct figure{
    width:20%;
    padding:5px;
    margin:10px;
    display: flex;
    flex-flow: row wrap;
    justify-content: space-around;
    border:1px solid #eee;
    border-radius: 5px;
    box-sizing: border-box;

}

.Classifyproduct figure img{
    width:80px;
    height:80px;
}
.Classifyproduct figure dl{
    width:50%;
    text-align: left;
    font-size: 14px;
}
.Classifyproduct figure dl dt{
    color:#b82424;
    margin-bottom:10px;
}
.Classifyproduct figure dl dd{
    color:#1D8ce0;
}
.Classifyproduct figure dl dd:last-child{
    float: right;
}
.allPrice{
    width:100%;
    margin: 20px 0;
    text-align: center;
    box-sizing: border-box;
}
.allPrice b{
    padding: 0 10px;
    font-size: 26px;
    color: #b82424;
}
</style>

