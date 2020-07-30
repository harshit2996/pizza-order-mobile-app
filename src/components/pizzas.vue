<template>
	<div>
		<q-drawer v-model="rd" side="right" bordered>
				<div v-if="cart.length">
					<q-scroll-area class="absolute-full">
						<div
							v-for="(item,i) in cart"
							:key="i"
							>
							<q-card 
								class="q-ma-md"
								>
								<q-card-section horizontal>
									<!-- <div class="column fit q-pa-none"> -->
										<q-img
											class="col-4 q-pa-none"
											:src="'https://pizzalove.herokuapp.com/img/'+item.image"
											contain
											>
										</q-img>
										<q-separator vertical inset></q-separator>
										<q-card-section class="q-pa-xs">
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

										</q-card-section>
									<!-- </div> -->
								</q-card-section>
							</q-card>
						</div >
						<div  class="text-center" v-text="'Pay : € ' + bill"></div >
						<div  class="text-center">
							<!-- <q-form method="POST" action="/order/create" enctype='multipart/form-data/application/json'> -->
								<!-- <input type="hidden" name="_token" :value="csrf"> -->
								<!-- <input type="hidden" name="order" :value="order"> -->
								<q-btn type="a" href="/#/order/create" color="primary">Buy Now</q-btn>
							<!-- </q-form> -->
						</div >
					</q-scroll-area>
				</div>
				<div v-else class="absolute-full">
					<div class="column full-height  justify-center">
						<div class=" text-center">
							<q-icon name="mdi-cart-off" size="md"></q-icon>
						</div >
						<div  class=" text-center">
							Your Cart is empty
						</div >
						<div  class=" text-center subtitle">
							Please add items and view here
						</div >

					</div >
				</div>
		</q-drawer>
		<div class="col start col-auto">
			<div class="text-center"><h2 class="q-ma-xs">Pizza Menu</h2></div >
			<div class="text-center"><span>Just Add to Cart and Order. It's that simple</span></div >
		</div>
		<div class="col">
			<div class="row full-height">
				<div class="col text-center">
					<div class="row justify-around">
						<div
							v-for="(pizza,i) in pizzas"
							:key="i"
							class=""
							>
					
						<q-card 
							@mouseover="pizza.hover=true"
							@mouseleave="pizza.hover=false"
							:class="pizza.hover?'pizza-card shadow-24 q-ma-md':'pizza-card q-ma-md'"
							>
							<div class="row full-height">
								<div class="column fit q-pa-none">
									<q-img
										:src="'https://pizzalove.herokuapp.com/img/'+pizza.image"
										height="175"
										>
										<div class="col absolute-bottom text-h6">
											{{ pizza.pizza_name }}
										</div>
									</q-img>
									<div class="col full-height">
										<div class="column full-height justify-between">
											<div class="col">
												<div class="column full-height justify-center">
													<div class="row items-center justify-center">
														{{ pizza.pizza_desc }}
													</div>
													<div class="row items-center justify-center">
														<div class="col">
															<div class="row justify-center items-center">
																<span>Qty :
																<q-btn flat size="xs" round @click="decQuantity(i)" icon="mdi-minus" />
																<span>{{ pizza.quantity }}</span>
																<q-btn flat size="xs" round @click="incQuantity(i)" icon="mdi-plus" />
																</span>
															</div>
														</div>
														<div class="col">
															<div class="row justify-center items-center">
																<span>Price :  € {{ pizza.price }}</span>
															</div>
														</div>

													</div>
												</div>
											</div>
											<div class="col col-auto q-pa-none">
												<q-btn color="primary" class="full-width" @click="addtocart(pizza,i)">Add to Cart</q-btn>
											</div>
										</div>
									</div>
								</div>
							</div>
						</q-card>
						</div>
					</div>
				</div>
			</div>
		</div>	
	</div>
</template>

<script>

import axios from 'axios'

export default {
	// props:['rdrawer','nitems'],

	data(){
		return{
			// rd:false,
			pizzas:[],
			cart:[],
			bill:0,
			order:{
				items:[],
				bill_amount:0,
			},
			csrf: document.querySelector('meta[name="csrf-token"]').getAttribute('content'),
			updated:[],      
			
		}
	},
	

	props:['numItems','rightdrawer'],

  computed: {
		rd:{
      get(){return this.rightdrawer},
      set(rd){this.$emit('update:rightdrawer', rd)}
		},
    
    numi: {
      get(){return this.numItems},
      set(numi){this.$emit('update:numItems', numi)}
		},
		
	},
			

	created(){

		this.order=JSON.parse(this.$store.state.order)
		if(this.order.items!=""){
				this.order.items.forEach(element => {
				this.cart.push(element)
			});
		}

		this.bill=this.order.bill_amount


		const p=[]

		axios.get('http://pizzalove.herokuapp.com/getpizza')
		.then(res=>{
			// console.log(res)
			let existing=false
			res.data.forEach(pizza => {
				pizza.quantity=0
				this.cart.forEach(item => {
					if(item.pizza_id==pizza.pizza_id){
						pizza.quantity=item.quantity
						existing=true	
					}
				})
				pizza.hover=false
				p.push(pizza)
				this.updated.push({
					flag:false,
					text:'Add to Cart'
				})

			})
		})
		.catch(err=>{
			console.log(err.response)
		})
		this.pizzas=p
		

		// console.log(p,this.hover)
	},

	methods:{
		createOrder(){
			let totalprice=0
			let order = {
				items:[],
				bill_amount:0
			}
			this.cart.forEach(pizza => {
				order.items.push(pizza)
				totalprice+=pizza.quantity*pizza.price				
			})
			totalprice= Math.ceil(totalprice * Math.pow(10, 2)) / Math.pow(10, 2);
			order.bill_amount=totalprice
			this.bill=totalprice
			// let cart = this.cart
			
			// this.order=(order)
			// this.cart=JSON.stringify(cart)
			this.order=JSON.stringify(order)
			this.$store.commit('mutate',{item:'order',newState: this.order})
			// this.$store.commit('mutate',{item:'cart',newState: this.cart})
			
		},

		addtocart(pizza,i){
			let existing=false
			// console.log(i)
			if(this.cart.length!=0){
				if(pizza.quantity>0){	
					// console.log(this.cart)
					this.cart.forEach((c,j)=>{
						if(c.pizza_id==pizza.pizza_id){
							this.cart[j].quantity=this.pizzas[i].quantity
							existing=true
						}
					})
					if(!existing)
						this.cart.push(pizza)
				}

				this.updated[i].flag=true
				this.updated[i].text='Update Quantity'
				this.createOrder()
			
			}else{
					this.cart.push(pizza)
					this.updated[i].flag=true
					this.updated[i].text='Update Quantity'
					this.createOrder()
			}
			let numi=0
			this.cart.forEach(element => {
				numi+=element.quantity
			})
			this.numi=numi			
		},


		incQuantity(i){
			this.pizzas[i].quantity+=1
		},

		decQuantity(i){
			
			
			if(this.pizzas[i].quantity!=0){
				this.pizzas[i].quantity-=1
				this.cart.forEach((item,j) => {
					if(item.pizza_id==this.pizzas[i].pizza_id)
						item.quantity=this.pizzas[i].quantity					
					if(this.cart[j].quantity==0){
						this.cart.splice(j,1)
						this.updated[i].flag=false
						this.updated[i].text='Add to Cart'
					}
				});
			}
			let numi=0
			this.cart.forEach(element => {
				numi+=element.quantity
			})
			this.numi=numi
			this.createOrder()

		},

	}


}
</script>

<style scoped>
.pizza-card{
	height: 300px;
	width:250px;
}


</style>