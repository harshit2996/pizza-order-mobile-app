<template>
  <q-page class="q-pa-md fit">
    <div class="row justify-between text-center q-pa-md">
      <div class="col-sm-auto text-h4">
        Enter your Details for Shipping
      </div>
      <div class="col-auto self-end">
        <q-btn type="a" href="#/" color="primary">Back</q-btn> 
      </div>
    </div>
    <!-- <q-separator></q-separator> -->
    <div class="column fit">
      <q-form ref="form" action="/order" method="POST" >
        <!-- <input type="hidden" name="_token" :value="csrf"> -->
        <input type="hidden" name="order" :value="orderstring">
        <input type="hidden" name="bill" :value="bill">

        <div class="co q-ma-lg">
          <div class="row q-ma-md">Personal Details</div>
          <q-separator class="q-mb-lg"></q-separator>
          <div class="row">
            <div class="col-12 col-sm-4">
              <q-input class="q-mx-sm fit"  dense outlined v-model="name" name="name" label="Name" prepend-inner-icon="mdi-account"  :rules="[rules.required]"></q-input>
            </div>
            <div class="col-12 col-sm-4">
              <q-input class="q-mx-sm fit"  dense outlined v-model="phone" name="phone" label="Phone" prepend-inner-icon="mdi-phone"  :rules="[rules.required]"></q-input>
            </div>
            <div class="col-12 col-sm-4">
              <q-input class="q-mx-sm fit"  dense outlined v-model="email" name="email" label="Email" prepend-inner-icon="mdi-gmail"  :rules="[rules.required,rules.email]"></q-input>
            </div>
          </div>

        </div>
        <div class="col q-ma-lg">
          <div class="row q-ma-md">Address Details</div>
          <q-separator class="q-mb-lg"></q-separator>
          <div class="row">
            <div class="col-12 col-sm-6">
              <q-input class="q-mx-sm"  dense outlined v-model="house" name="house" label="House Number" prepend-inner-icon="mdi-home"  :rules="[rules.required]"></q-input>
            </div>
            <div class="col-12 col-sm-6">
              <q-input class="q-mx-sm"  dense outlined v-model="street" name="street" label="Street" prepend-inner-icon="mdi-map"  :rules="[rules.required]"></q-input>
            </div>
          </div>
          

          <div class="row">
            <div class="col fit">
              <q-input class="q-mx-sm"  dense outlined v-model="city" name="city" label="City" prepend-inner-icon="mdi-map-marker"  :rules="[rules.required]"></q-input>
            </div>
            <div class="col fit">
              <q-input class="q-mx-sm"  dense outlined v-model="zipcode" name="zipcode" label="Zip Code" prepend-inner-icon="mdi-mailbox"  :rules="[rules.required]"></q-input>
            </div>
          
          </div>
        </div>

        <div class="col q-ma-lg">
          <div class="row items-center q-ma-lg">
            <div class="col">Order Review</div>
            <div class="col">
              <div class="row justify-end">
                <q-btn type="a" href="#/" color="primary">Back</q-btn> 
              </div>
            </div>
          </div>
        
          <q-separator class="q-mb-lg"></q-separator>
          <q-carousel
            transition-prev="slide-right"
            transition-next="slide-left"
            swipeable
            animated
            control-color="primary"
            navigation
            arrows
            height="200px"
            class="bg-grey-2 q-pa-none"
              

            v-model="cslide"      
            >
            
            <q-carousel-slide v-for="(item,i) in final_order.items"
            :key="i" :name="i" class="row items-center justify-center">
              <q-card 
                class="q-ma-none item-card"
                >
                <q-card-section horizontal>
                  <!-- <div class="column fit q-pa-none"> -->
                    <q-img
                      class="col-4 q-pa-none"
                      style="height: 150px; max-width: 300px"
                      :src="'https://pizzalove.herokuapp.com/img/'+item.image"
                      contain
                      >
                    </q-img>
                    <q-separator vertical inset></q-separator>
                    <q-card-section class="q-pa-xs row items-center">
                      <div class="column">
                        <div class="col text-subtitle2">
                          {{ item.pizza_name }}
                        </div>
                        <div class="col q-pa-none">
                          <div class="column full-height justify-around">
                            <div class="col">
                              <div class="column full-height">
                                <div class="row items-center justify-center text-body2">
                                  {{ item.pizza_desc }}
                                </div>
                                <div class="row items-center justify-center">
                                  <div class="col q-pa-none">
                                    <div class="row items-center">
                                      <span>Qty :
                                      <!-- <q-btn flat size="xs" round @click="decQuantity(i)" icon="mdi-minus" /> -->
                                      <span>{{ item.quantity }}</span>
                                      <!-- <q-btn flat size="xs" round @click="incQuantity(i)" icon="mdi-plus" /> -->
                                      </span>
                                    </div>
                                  </div>
                                  <div class="col">
                                    <div class="row items-center">
                                      <span>Price :  € {{ item.price }}</span>
                                    </div>
                                  </div>

                                </div>
                              </div>
                            </div>
                            <div class="col col-auto q-pa-none">
                              <!-- <q-btn color="primary" class="full-width" @click="addtocart(item,i)">Add to Cart</q-btn> -->
                            </div>
                          </div>
                        </div>
                      </div>

                    </q-card-section>
                  <!-- </div> -->
                </q-card-section>
              </q-card>
            </q-carousel-slide>
          </q-carousel>

        </div>
        
        
        <div class="row q-my-lg q-mx-md">
          <div class="column fit">
            <div class="row text-center"> Total Amount for Pizzas € : {{final_order.bill_amount}}</div>
            <div class="row text-center"> Dilevery Charges  € : {{charge.amount}} <span :class="final_order.bill_amount>20?'q-ml-md text-positive':'q-ml-md text-warning'"> {{charge.message}}</span></div>
            
            <div class="row justify-between items-center">
              <div class="column">
                <div class="row" v-text="'Total amount to be paid € :'+bill"></div>
              </div>
              <div class="column">
                <div class="row justify-end">
                  <q-btn color="primary" type="button" @click="placeorder"> Proceed to Pay</q-btn>
                </div>
              </div>
            </div>
          </div>
        </div>


      </q-form>
    </div>

    <success :success="success" :message="'Your Order has been placed Successfully'" :location="'#/'"></success>


  </q-page>
</template>

<script>

import success  from 'components/success.vue'
import axios from 'axios'


export default {
  
  components:{success},

  data(){
    return{
      success:false,
      cslide:0,
      final_order:[],
      charge:{
        amount:0,
        message:'Delivery free for orders of more than € 20.00'
      },
      csrf: document.querySelector('meta[name="csrf-token"]').getAttribute('content'),
      bill:0,
      orderstring:[],
      name:'',
      phone:'',
      email:'',
      address:'',
      house:'',
      street:'',
      city:'',
      zipcode:'',
      rules: {
        required: value => !!value || 'Required.',
        min: v => v.length >= 8 || 'Min 8 characters',
        email:value=>{
          const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
          return pattern.test(value) || 'Invalid e-mail.'
        },
      },

    }
  },

  mounted(){
    
    this.final_order=JSON.parse(this.$store.state.order)
    if(this.final_order.bill_amount>20.00){
      this.charge.amount=0
      this.charge.message='Free'
    }
    else if(this.final_order.bill_amount>15.00){
      this.charge.amount=0.50
      this.charge.message='Delivery free for orders of more than € 20.00'
    }
    else if(this.final_order.bill_amount>10.00){
      this.charge.amount=0.75
      this.charge.message='Delivery free for orders of more than € 20.00'
    }
    else{
      this.charge.amount=1.5
      this.charge.message='Delivery free for orders of more than € 20.00'
    }

    this.bill=this.final_order.bill_amount+this.charge.amount
    this.orderstring=JSON.stringify(this.final_order)
    // console.log(this.orderstring)
  },

  methods:{
    validate(){
      this.$refs.form.validate()
    },

    placeorder(){
      if(this.$refs.form.validate()){
        const formData=new FormData()
        formData.append('name',this.name)
        formData.append('phone',this.phone)
        formData.append('email',this.email)
        formData.append('house',this.house)
        formData.append('street',this.street)
        formData.append('city',this.city)
        formData.append('zipcode',this.zipcode)
        formData.append('order',this.orderstring)
        formData.append('bill',this.bill)

        console.log(formData)
  
        axios.post('https://pizzalove.herokuapp.com/order',formData)
            .then(res=>{
              if(res.data!=''){
                this.message=res.data
              }
              else{
                this.success=true
              }
            })
            .catch(err=>{
              console.log(err.response)
            })

      }
    },

    
  }

}
</script>

<style>
.item-card{
  height: 150px;

}
</style>