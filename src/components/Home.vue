<template>
  <div id="home-customer">
    <ul class="collection with-header">
      <li class="collection-header"><h4>Customers</h4></li>
      <li v-for="customer in customers" v-bind:key="customer.id" class="collection-item">
        <div class="chip">{{customer.id}}</div>
       {{customer.first_name}} {{customer.last_name}}
         <router-link class="secondary-content" v-bind:to="{ name: 'view-customer', params: { customer_id: customer.customer_id }}"><i class="fa fa-angle-double-down"></i></router-link>
      </li>
    </ul>
    <div class="fixed-action-btn">
      <router-link to="/new/customer" class="btn-floating btn-large red">
        <i class="fa fa-plus"></i>
      </router-link>
    </div>
  </div>
</template>

<script>
import db from './firebaseInit.js'
export default {
  name: 'dashboard',
  data() {
    return {
      customers: []
    }
  },
  created () {
    db.collection('customers').orderBy('customer_id').get().then(querySnapshot => {
      querySnapshot.forEach(doc => {
        const data = {
          'id': doc.id,
          'customer_id': doc.data().customer_id,
          'first_name': doc.data().first_name,
          'last_name': doc.data().last_name,
          'phone': doc.data().phone,
          'email': doc.data().email,
          'social': doc.data().social,
          'returning_customer': doc.data().returning_customer
        }
        this.customers.push(data)
      })
    })
  }
}
</script>
