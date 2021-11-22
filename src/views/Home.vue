<template>
<main>
  <div>
  <div class="header">
    <header>
      <!--<img class="headerimg" src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSUocAZdiTSKjnaj5BeZbPr8O0sbgwLGFrRpA&usqp=CAU" alt="">-->
      <h1 id="headertext"> Välkommen till BurgerOnline </h1>
    </header>
  </div>
  <div class="Burgers">
    <Burger v-for="burger in burgers"
            v-bind:burger="burger"
            v-bind:key="burger.name"
            v-on:orderedBurger ="addToOrder($event)"/>
  </div>
  </div>

<section id="contact" style="clear:left">
  <h1> Contact information </h1>
  <p>
    <label for="name"> Name and surname </label> <br>
    <input type="text" v-model="name" required="required" placeholder="Please enter name and surname">
  </p>
  <p>
    <label for="email"> E-mail </label> <br>
    <input v-model="email" required="required" placeholder="Please enter your e-mail">
  </p>
  <p>
    <label for="paymentMethod"> Payment Method </label> <br>
    <select v-model="paymentMethod">
      <option disabled value=""> Choose your preferred method of payment </option>
      <option v-for="pay in payment" v-bind:key="pay" :value="pay" > {{pay}} </option>
    </select>
  </p>
    <label for="Gender"> Gender </label> <br>
    <div v-for="g in gender" v-bind:key="g">
    <input type="radio" name="g.txt" v-model="Gender" :value="g.txt">{{g.txt}}
    </div>

  <section id="mapContainer">
    <div id="map" v-on:click="setLocation">
      <div v-bind:style="{left:location.x + 'px', top:location.y + 'px'}">T</div>
    </div>
  </section>
    <button type="submit" v-on:click="submit"><img src="https://w1.pngwing.com/pngs/546/859/png-transparent-food-icon-delivery-icon-sushi-pizza-delivery-scooter-courier-food-delivery-text-thumbnail.png" style="width:20px"> Submit Order </button>
</section>
</main>

</template>

<script>
import Burger from '../components/Burger.vue'
import menu from '../assets/menu.json'
import io from 'socket.io-client'

const socket = io();

/* Old creation of MenuItem and burgers, not used in endresult

function MenuItem(name, url, kCal, gluten, lactose) {
  this.Name = name;
  this.Url = url;
  this.kCal = kCal;
  this.gluten = gluten;
  this.lactose = lactose;
}
 console.log(MenuItem);

const burgers = [ {name: "Cheese", url: "https://www.burgerdudes.se/wp-content/uploads/2019/05/jettz_cheese_mar20_ny-rec.jpg", kCal: 400, gluten: true, lactose: true},
                {name: "Semla", url: "https://www.isof.se/images/18.282eed401758d09d82c1b86/1604476928798/semlor.jpg", kCal: 540, gluten: true, lactose: true},
                {name: "Badonkadonk", url: "https://www.humorbibeln.se/wp-content/uploads/sites/4/2018/12/burgarevisning.jpg", kCal: 1270, gluten: true, lactose: true},
]
console.log(burgers);
*/

export default {
  name: 'Home',
  components: {
    Burger
  },
  data: function () {
    return {
      burgers: menu,
      gender: [{txt:'Non-binary', val:0}, {txt:'Male', val:0}, {txt:'Female', val:0}, {txt:"Prefer not to disclose", val:0}],
      payment: ["Credit card", "Debit Card", "Swish", "Cash", "PayPal"],
      name: '',
      email: '',
      street: '',
      orderedBurgers: {},
      location: {x: 0,
                 y: 0}
    }
  },
  methods: {
    submit: function (){ /* What is submitted when customer is done with order and push the Submit order button.
      console.log(this.name, this.email, this.street, this.paymentMethod, this.Gender, this.orderedBurgers) */
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: {name: this.name, email: this.email, gender: this.Gender, payment: this.paymentMethod, x: this.location.x, y: this.location.y },
                                orderItems: [this.orderedBurgers]
                              }
                 );
      },
    getOrderNumber: function () { /* Generates order numbers */
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) { /* Old way of adding order to dispatcher view, not used */
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    },
    addToOrder: function(event){ /* Listens to added orders from Burger.vue*/
      this.orderedBurgers[event.name] = event.amount;
    },
    setLocation: function(event){ /* Location on map is saved using this function */
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
      y: event.currentTarget.getBoundingClientRect().top};
      this.location = {x: event.clientX - 10 -offset.x,
      y: event.clientY - 10 - offset.y}
    }
  }
}
</script>

<style>
.allergy {
  text-transform: uppercase;
}

body {
    font-family: arial;
    font-size: 12pt;
    border: 100pt;
}

submit:hover {
  background-color: blue;
  cursor: pointer;
}

.Burgers {
  display: grid;
  grid-template-columns: auto auto auto;
  margin: 10pt;
}

#burgers {
  overflow: auto;
  background-color: black;
  color: white;
  margin: 10pt;
  border: 2px dashed green;
  margin-top: 0pt;
  margin-bottom: 20pt;
}

/*.demo-content {
  position: relative;
}*/

#contact{
  margin: 10pt;
  margin-top: 0pt;
  padding-left: 10pt;
  border: 2px solid black;
}

.header {
  position: relative;
}

.header:before {
  margin: 10 pt;
  content: ' ';
  display: block;
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  opacity: 0.6;
  background-image: url('https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSUocAZdiTSKjnaj5BeZbPr8O0sbgwLGFrRpA&usqp=CAU');
  background-repeat: no-repeat;
  background-position: 50% 0;
  background-size: cover;
}

.header{
  overflow: hidden;
  margin: 10pt;
  padding-left: 10pt;
  text-align: center;
}

.headerimg{
  opacity: 0.6;
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: auto;
}

#laktos, #gluten{
  color: #ff5500;
}


input[type:radio]:hover{
  cursor: pointer;
}

/*
a, strong, em {
    color: #0099ff;
}

p.info,
#login p {
    width: 80%;
    margin: 1vw auto;
}
*/

#map div {
  position: absolute;
  background: black;
  color: white;
  border-radius: 10px;
  width:20px;
  height:20px;
  text-align: center;
}

#map {
  width: 1920px;
  height: 1078px;
  background: url("/img/polacks.jpg");
  position: relative;
  margin: 0;
  padding: 0;
  background-repeat: no-repeat;
  cursor: crosshair;
}

#mapContainer{
  width: 850pt;
  height: 800px;
  overflow:scroll;
  margin: 0 auto;
}
</style>
