<script setup lang="ts">
import { supabase } from '@/supabase';
import { ref } from "vue"

interface Order {
  name: string,
  id: string,
  created_at: string,
  client_id: string,
  price: number,
  address: string,
  city: string,
  zip_code: string,
  views: number
}
const orders = ref<Order[]>([])
const orderssecond = ref<Order[]>([])

const insertOrder = async () => {
  try {
    const { data, error } = await supabase
      .from('orders')
      .insert({
        address: 'plebania',
        zip_code: '82100',
        city: 'dro',
        name: 'xyz',
        client_id: '0826683e-75d7-4231-abf8-a05c6d032f29',
        price: 12
      })
    if (data) {
      console.log(data);
    }
  } catch (error) {
    console.log(error);
  }
}
const fetchOrdersById = async (id: string) => {
  try {
    const { data: orders, error } = await supabase
      .from('orders')
      .select('*')
      .eq('client_id', id)
    if (orders) {
      console.log(orders);
    }
  } catch (error) {
    console.log(error);
  }
}
const updateOrderPrice = async (id: string, newPrice: number) => {
  const { data, error } = await supabase
    .from('orders')
    .update({ price: newPrice })
    .eq('client_id', id)
    .select()

  if (data) {
    //console.log(data);
  }
  if (error) {
    console.log(error);
  }
}
const deleteOrderByZipCode = async (zip_code: string) => {

  const { error } = await supabase
    .from('orders')
    .delete()
    .eq('zip_code', zip_code)

  if (error) {
    console.log(error);
  }
}
const fetchOrdersSecond = async () => {
  try {

    let { data, error } = await supabase
      .from('orderssecond')
      .select(`
        *,
        clientssecond (
          *
        )
      `)

    if (data) {
      console.log(data);

    }
  } catch (error) {
    console.log(error);
  }
}
const fetchOrders = async () => {

  const { data, error } = await supabase
    .from('orders')
    .select('*')
  if (data) {
    console.log(data);
    orders.value = data

  }
  if (error) {
    console.log(error);
  }
}

const channel = supabase
  .channel('my_new_channel_for_orders')
  .on('postgres_changes',
    {
      event: '*',
      schema: 'public',
      table: 'orders',
    },
    (event) => {
      const { new: newOrder } = event;
      orders.value = orders.value.map(order => {
        if (order.id === newOrder.id) {
          return {
            ...order,
            ...newOrder
          }
        }
        return order
      })
      console.log(event);
    }
  )
  .subscribe()

const incrementViews = async () => {
  try {
    const { data, error } = await supabase.rpc('increment', {
      row_id: '08350d9f-fc71-4d70-9f74-cca79ac0256e'
    })
    if (data) {
      console.log("success")
      console.log(data)
    }
  } catch (error) {
    console.log(error)
  }
}

incrementViews()
fetchOrders()
//fetchOrdersSecond()
</script>

<template>
  <main class="flex items-center justify-between">
    <div class="container px-2 mx-auto my-8">
      Hello World
      <button class="block btn btn-primary" @click="insertOrder"> Insert order</button>
      <button class="block btn btn-primary" @click="fetchOrdersById('0826683e-75d7-4231-abf8-a05c6d032f29')"> Fetch
        order by id</button>
      <button class="block btn btn-primary" @click="updateOrderPrice('0826683e-75d7-4231-abf8-a05c6d032f29', 17)"> Upd
        price</button>
      <button class="block btn btn-primary" @click="deleteOrderByZipCode('98052')"> depete by zip code</button>

      <h1>Vanilla Orders</h1>
      <div v-for="order, index in orders" :key="index">
        <div v-if="order" class="grid grid-cols-5 gap-3 px-3 py-2 my-2 border rounded-lg shadow-md">
          <div v-if="order.name">{{ order.name }}</div>
          <div>${{ order.price || 0 }}</div>
          <div v-if="order.address">{{ order.address }}</div>
          <div v-if="order.zip_code">{{ order.zip_code }}</div>
          <div v-if="order.city">{{ order.city }}</div>
          <div v-if="order.views">{{ order.views }}</div>
        </div>
      </div>
    </div>
  </main>
</template>
