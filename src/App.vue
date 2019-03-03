<template>
  <div id="app">

    <div class="container">
    <button class="btn btn-success mt-5 mb-5"
        @click="addNewFriendForm">
        Add a Person
    </button>
     

    <div>
      <input type="number" class="form-control mb-2" placeholder="NetTotal (Inc Gst & Service Charge)"
        v-model="netTotal">
    </div>

    <div v-for="(friend,index) in friends" :key="index">
            <div class="card-body">
                <span class="float-right" style="cursor:pointer"
                  @click="deleteFriendForm(index)">  
                    X
                </span>

                <h4 class="card-title">{{ index + 1}}.New Person </h4>
                 <div class="friend-form">
                      <input type="text" class="form-control mb-2" placeholder="Name"
                      v-model="friend.name">
                     <input type="text" class="form-control mb-2" placeholder="Price"
                     v-model="friend.price" id="demo"  v-on:keyup.enter="onEnterClick">
                 </div>
            </div>

        </div>



    <button class="btn btn-success mt-5 mb-5"
        @click="spliter" v-on:click="isHidden = false">
        Split
    </button>
    

    <hr>
    <hr>
    <div id="invoice-POS" v-if="!isHidden">
        <div class="col-xs-12 col-sm-8 col-sm=offset-2 col-md-6 col-md-offset-3">
          <div class="panel panel-default">
             <div class="panel-heading">
               <h4> Updated Bill</h4>
          </div>
          <div v-for="(friend,index) in friends" :key="index" >
              <p>{{friend.name}}  has to pay MVR{{friend.splity}} </p>

          </div>
         </div>
        </div>
    </div>
</div>
   
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      netTotal :'',
      eachPay: 0,
      isHidden: true,
      split:0,

           friends: [{
               name:'',
               price:'',
               splity:''
             }]
    }

  },
  methods: {
            addNewFriendForm () {
                 this.friends.push({
                     name:'',
                     price:''
                 })
            },

            deleteFriendForm(index) {
                this.friends.splice(index, 1)  
            },

            spliter(){
                        let sum = this.friends.reduce((prev, cur) => {
                         return prev + parseInt(cur.price);               
                           },0); 
                        for(var i = 0; i < this.friends.length; ++i){
                            this.eachPay = parseInt(this.friends[i].price);
                            this.split = (this.eachPay/sum)*this.netTotal;
                            this.friends[i].splity = this.split.toFixed(2);
                            console.log(`${this.friends[i].name} has to pay: MVR ${this.split.toFixed(2)}`);
                         }

                  },
                  onEnterClick() {
                        for(var i = 0; i < this.friends.length; ++i){
                            let p = this.friends[i].price;
                            let val = p.split('+');
                            val = parseInt(val.reduce((a,c)=> a + Number(c), 0));
                            this.friends[i].push({
                              price : val,
                            })
                         }
                  },

}
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

h4 {
  text-align: center;
}
ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

#invoice-POS{
  text-align: center;
  box-shadow: 0 0 1in -0.25in rgba(0, 0, 0, 0.5);
  padding:2mm;
  margin: 0 auto;
  width: 150mm;
  background: #FFF;
  }
</style>
