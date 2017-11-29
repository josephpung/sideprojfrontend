<template>

<div id="show-orders">
  <div class="" v-for="order in orders" >
    {{order.order_id}}
    <div class="" v-for="dishes in order.order_items" >
      {{dishes.name}}
    </div>
  </div>
  <transition name="modal" v-if="seen" >
    <div class="" v-for="order in orders" :key="order.id" >
    <div class="modal-mask">
      <div class="modal-wrapper">
        <div class="modal-container">

          <div class="modal-header">
            <slot name="header">
              default header
            </slot>
          </div>

          <div class="modal-body">
            <slot name="body">
              <form>
                <label>{{order.order_id}}</label><br />
                <div class="" v-for="dishes in order.order_items" >
                  {{dishes.name}}
                <label></label>
                <input type="radio" id="plus" value="plus" v-model="picked">
                <label for="plus">Good</label>
                <input type="radio" id="minus" value="minus" v-model="picked">
                <label for="minus">Bad</label>
                </div>
                <br>
                <span>Comments</span>
                <input type="text" ref="my_input">
                <button @click.prevent="getFormValues()">Get values</button>
              </form>
              Output: {{ output }}
            </slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              default footer
              <button class="modal-default-button" @click="seen = false">
               Submit
              </button>
            </slot>
          </div>
        </div>
      </div>
    </div>
  </div>
  </transition>

  <h1>All Orders</h1>
  <div class="single-order"></div>
  <table align="center">
  <tr>
    <th>Order_id</th>
    <th>Delivery Date</th>
    <th>Delivery Time</th>
    <th>Feedback</th>
  </tr>
  <tr v-for="order in orders">
    <td>{{order.order_id}}</td>
    <td>{{order.delivery_date}}</td>
    <td>{{order.delivery_time}}</td>
    <td>
      <button v-if=!order.feedback_submitted type="button" @click="seen = true">Review</button>
      <label v-else=order.feedback_submitted for=""> Thank you for your feedback</label>
    </td>
  </tr>
</table>
</div>
</template>

<script>

export default{

  data(){
    return{
      orders: [],
      seen: false,
      picked: false,
      output: ''

    }
  },
  methods:{
    submit: function(){
      alert("Open modal" + vm.toggle)
      // this.$http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
    },
    getFormValues () {
      // this.picked = vm.picked
      this.output = this.$refs.my_input.value + vm.picked
    }

  },
  created(){
    this.$http.get("https://sideproj.herokuapp.com/orders")
    .then(function(data){
      this.orders = data.body.orders
      console.log(this.orders[1].order_items);
      })
  }
}

</script>
<style>
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, .5);
  display: table;
  transition: opacity .3s ease;
}

.modal-wrapper {
  display: table-cell;
  vertical-align: middle;
}

.modal-container {
  width: 300px;
  margin: 0px auto;
  padding: 20px 30px;
  background-color: #fff;
  border-radius: 2px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
  transition: all .3s ease;
  font-family: Helvetica, Arial, sans-serif;
}

.modal-header h3 {
  margin-top: 0;
  color: #42b983;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

/*
 * The following styles are auto-applied to elements with
 * transition="modal" when their visibility is toggled
 * by Vue.js.
 *
 * You can easily play with the modal transition by editing
 * these styles.
 */

.modal-enter {
  opacity: 0;
}

.modal-leave-active {
  opacity: 0;
}

.modal-enter .modal-container,
.modal-leave-active .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}

</style>
