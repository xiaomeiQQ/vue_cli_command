<style>
      .wrapper{
          width: 100%;
          height: 80vh;
          /* display: flex; */
      }
      main{
          display: inline-block;
          width: calc(100vw - 21rem);
          overflow: scroll;
      }
      aside{
          display: inline-block;
          width: 20rem;
          vertical-align: top;
      }
      .productContainer{
          display: inline-flex;
      }
      #app{
          display: block;
      }
      img{
          width: 100px;
      }
</style>
<template>
  <div class="wrapper">
        <main>
            <header>
                search: <input type="text" v-model="search"><br>
                篩選出大於: <input type="number" v-model="min">-<input type="number" v-model="max">$的商品
            </header>
            <template v-if="load">loading...</template>
            <template v-else>
                <div  class="productContainer">
                    <div v-for=" (item,index) in product" :key="item.id">
                        <p>{{item.title}}</p>
                        {{parseRating(item.rating)}}
                        <!-- <template v-if="item.rating"> -->
                            <!-- <p>{{item.rating}}</p> -->
                            <!-- <p>{{item["rating"]["rate"]}}</p> -->
                            <!-- <span v-for="count in item['rating']['rate']">{{count}}</span> -->
                            <!-- <span v-for="count in parseInt(item['rating']['rate'])">⭐</span> -->
                            <!-- <span v-for="count in (5-parseInt(item['rating']['rate']))">✩</span> -->
                            <!-- <p> 評論數: {{item.rating.count}}</p> -->
                        <!-- </template> -->
                        <img :src="item.image" v-bind:alt="item.title">
                        <p>${{parsePrice(item.price)}}元</p>
                        <div>
                            <button @click="reduceCount(index,item)">-</button>
                            <!-- <input type="text" v-model.number="count"> -->
                            <input type="number" min="0" v-model="count[index]">
                            <button @click="addCount(index,item)">+</button>
                        </div>
                    </div>
                </div>
            </template>
        </main>
        <aside>
            訂單
            <ul>
                <li v-for="item in order" :key="item.id">
                    {{item.title}}:{{item.count}}
                </li>
            </ul>
            <!-- {{order}} -->
            <p>總價: {{total}}元</p>
        </aside>
  </div>
</template>
<script>
        // methods可以帶參數 computed不行
        export default {

          data(){
              return{
                  load: false,
                  source:[],
                  sourceCount:0,
                  order:[],
                  count:[],
                  search:'',
                  min:0,
                  max:0
              }
          },
          created(){
              console.log('created')
              this.getResourse()
          },
          computed:{
              product(){
                  let cache=this.source
                  if(this.min>0){ //篩選最小值
                      cache=cache.filter((item,index)=>{
                          return item.price>this.min
                      })
                  }
                  if(this.max>0){ //篩選最大值
                      cache=cache.filter((item,index)=>{
                          return item.price<this.max
                      })
                  }
                  if(this.search !==''){ // 篩選相符標題
                      cache=cache.filter((item,index)=>{
                          return item.title.includes(this.search)
                      })
                  }
                  return cache
              },
              // return this.source
              total(){
                  if(this.product.length>0){
                      let total=0
                      for (const index in this.product){
                          total +=this.count[index]*this.product[index]['price']
                      }
                      return parseInt(total)
                  }else{
                      return 0
                  }
              }
          },
          watch:{
              // 監聽data或computed
              total:{
                  handler: function(newVal,oldVal) {
                      if(newVal>1000){
                          // console.log(newVal);
                          // console.log(oldVal);
                          alert('你要餓死了')
                      }
                  }
                  // deep: true
              }
          },
          methods:{
              parseRating(rating){
                  if(!rating) return null
                  let starStr=''
                  const star=parseInt(rating.rate)
                  for(let i=0;i<star;i++){
                      starStr+='⭐'
                  }
                  return `${starStr} 評論數:${rating.count}`
              },
              parsePrice(price){
                  return `TWD ${price*33}`
              },
              getResourse(){
                  this.load=true
                  fetch('https://fakestoreapi.com/products')
                  .then(res=>res.json())
                  // .then(json=>console.log(json))
                  .then(json=>{
                      this.source=json
                      for(let item in this.product){
                          this.count.push(0)
                      }
                      this.load=false
                  })
                  //初始化商品數量
              },
              addCount(index,item){
                  this.count[index]+=1
                  // find -> 找出東西
                  // some->判斷訂單面有沒有(orderItem)存在新增的商品(item) (true/false)
                  // const exits=this.order.some(orderItem=>{return orderItem.id ===item.id})
                  // console.log(exits);
                  // 如果訂單清單裡有這個東西，就將同id的商品增加數量count
                      // 取訂單商品的順序
                  const prodIndex=this.order.findIndex(orderItem=>{
                      return orderItem.id === item.id
                  })
                  console.log(prodIndex);
                  if(prodIndex>=0){
                      // // 如果訂單清單裡有這個東西，就將同id的商品增加數量count
                      // // 取訂單商品的順序
                      // const prodIndex=this.order.findIndex(orderItem=>{
                      //     return orderItem.id === item.id
                      // })
                      // console.log(prodIndex);
                      console.log(this.order[prodIndex]);
                      // // 將訂單數量+1
                      // // this.order[prodIndex]['count'] = this.order[prodIndex]['count']+1
                      this.order[prodIndex]['count']+=1
                  }else{
                      // prodIndex結果會是-1
                      // 如果訂單清單裡沒有這個東西，就新增一個訂單商品
                      // 加
                      this.order.push({
                          id: item.id,
                          title: item.title,
                          price: item.price,
                          count:1 //init
                      })
                  }
                  this.setStorage()
              },
              reduceCount(index,item){
                  console.log(item);
                  if(this.count[index]<=0) return
                  this.count[index]-=1
                  // 減
                  const prodIndex=this.order.findIndex(orderItem=>{
                      return orderItem.id === item.id
                  })
                  if(prodIndex<0) return;
                  if(this.order[prodIndex]['count']>1){
                      // this.order[prodIndex]['count'] = this.order[prodIndex]['count']-1
                      this.order[prodIndex]['count']-=1
                  }else{
                      this.order.splice(prodIndex,1)
                  }
                  this.setStorage()
              },
              getStorage(){
                  let data =localStorage.getItem('xiaomeiQQ');
                  // JSON.parse() -->將字串轉為陣列或物件
                  // 宣告一個常數 data 將localstorage中取得的資料賦值給常數
                  data=JSON.parse(date)
                  this.order=data? data:[]
              },
              setStorage(){
                  // JSON.stringify() -->將陣列或物件轉字串
                  // 宣告一個常數 data 將其值轉換為字串(Value)存在key為"xiaomeiQQ"localstorage
                  const data=JSON.stringify(this.order);
                  localStorage.setItem('xiaomeiQQ',data);
              }
          },
          mounted(){
              console.log('mounted');
          },
          beforeUnmount(){
              alert('beforeUnmount')
              console.log('beforeUnmount');
          },
}
  </script>
  