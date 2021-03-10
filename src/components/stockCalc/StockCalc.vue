<template>
  <div id="calcApp">
    <h1>股票買進紀錄</h1>
    <form class="buyStockForm">
      <!-- 輸入欄位 -->
      <input class="buyStockInput" placeholder="請輸入股號" type="text" name="stockSymbol" v-model="stockSymbol" @keypress="isShow=true">
      <input class="buyStockInput" placeholder="請輸入價位" type="text" name="strikePrice" v-model="strikePrice">
      <input class="buyStockInput" placeholder="請輸入張數" type="text" name="boardLot" v-model="boardLot">
      <!-- 輸入顯示 -->
      <div class="displayInput" v-show="isShow">
        <span >{{ stockSymbol }}</span>
        <span class="displayInput-style">價格</span>{{ strikePrice }}<span class="displayInput-style">元</span>
        <span >{{ boardLot }}</span><span class="displayInput-style">張</span>
      </div>
      <!-- 按鈕 -->
      <button class="buyStockBtn" type="button" @click="addStock(stockSymbol,strikePrice,boardLot)">送出</button>
    </form>

    <!-- 顯示紀錄區 -->
    <div class="stockHolding" v-for='(data, index) in inputCost' :key='index'>
      <div class="stockHolding-info">
        <span class="stockHolding-info-style">股號</span>
        <span>{{data.stockSymbol}}</span> 
        <span class="stockHolding-info-style">買進價</span>
        <span>{{data.strikePrice}}</span>
        <span class="stockHolding-info-style">張數</span>
        <span>{{data.boardLot}}</span>
      </div>
      <div class="stockHolding-cost">
        <span class="stockHolding-cost-style">總價</span>
        <span>{{data.strikeCost}}</span>
        <i class="fa fa-pencil" v-on:click="edit(index)"></i>
        <i class="fa fa-minus-circle" v-on:click="remove(index)"></i>
      </div>
    </div>
    <!-- 修改資料頁modal -->
    <div class="stockEditModal"  v-show="isEdit">
      <div class="stockEditModal-content">
        <i class="close fa fa-close" @click="isEdit=false"></i>
        <input type="text" placeholder="請輸入張數" class="stockEdit" name="editBoardLot" v-model.number="editBoardLot">
        <input type="text" placeholder="請輸入價位" class="stockEdit" name="editStrikePrice" v-model.number="editStrikePrice">
        <i class="fa fa-send" @click="save()"></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name:'StockCalc',
  title:'Stock Trading Note',
  data(){

    return{
      stockSymbol:'',
      strikePrice:'',
      boardLot:'',
      isShow:false,
      isEdit:false,
      inputCost:[],
    }
  },

  methods:{

    addStock(stockSymbol,strikePrice,boardLot){
      // 手續費折數
      let commissionFee = 0.38
      // 買入費用
      const buyCost = Math.floor(strikePrice * boardLot * 1000 * 0.001425 * commissionFee)
      // 存資料
      this.inputCost.push({
        "stockSymbol": stockSymbol,
        "boardLot" : boardLot,
        "strikePrice" : strikePrice,
        "strikeCost" : strikePrice * boardLot * 1000 + buyCost,
      })
      // 回復初始值
      this.stockSymbol=''
      this.strikePrice=''
      this.boardLot=''
      this.isShow=false
    },

    // 移除欄位
    remove(id){
      this.inputCost.splice(id,1)
    },
    // 顯示編輯頁面
    edit(id){
      this.isEdit=true
      // 複製原始資料來處理
      this.editStock=Object.assign({}, this.inputCost[id])
      // 存進原始資料的 id
      this.editStock.target = id
    },

    // 存資料
    save(){
      // 抓 id 來用
      let id = this.editStock.target
      // 手續費折數
      let commissionFee = 0.38
      // 修改後的買入費用
      const buyCost = Math.floor(this.editStrikePrice * this.editBoardLot * 1000 * 0.001425 * commissionFee)
      // 存入修改的資料
      this.inputCost[id].strikePrice=this.editStrikePrice
      this.inputCost[id].boardLot=this.editBoardLot
      // 重新計算
      this.inputCost[id].strikeCost=this.editStrikePrice * this.editBoardLot * 1000 + buyCost
      // 清除編輯區與隱藏modal
      this.editStock={}
      this.isEdit=false
    },
  },


}
</script>

<style lang="scss" scoped>

@import "https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css";

@import url('https://fonts.googleapis.com/css2?family=Fjalla+One&family=Noto+Sans+TC:wght@400;700&display=swap');

@import '@/components/stockCalc/StockCalc.scss';

</style>