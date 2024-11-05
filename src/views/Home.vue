<script setup lang="ts">
import { supabase } from '@/supabase';
import { ref } from "vue"

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
      console.log(data);
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
</script>

<template>
  <main class="flex items-center justify-between">
    <div class="container px-2 mx-auto my-8">
      Hello World
      <button class="block btn btn-primary" @click="insertOrder"> Insert order</button>
      <button class="block btn btn-primary" @click="fetchOrdersById('0826683e-75d7-4231-abf8-a05c6d032f29')"> Fetch order by id</button>      
      <button class="block btn btn-primary" @click="updateOrderPrice('0826683e-75d7-4231-abf8-a05c6d032f29', 17)"> Upd price</button>
      <button class="block btn btn-primary" @click="deleteOrderByZipCode('98052')"> depete by zip code</button>
    </div>
  </main>
</template>

<!-- 
//fetchOrders()

/* 
 interface Order {
  name: string,
  id: string,
  created_at: string,
  client_id: string,
  price: number,
  address: string,
  city: string,
  zip_code: string,
}

const orders = ref<Order[]>([])

const pagination = ref(9)

const fetchOrders = async () => {
   try {
     const { data, error } = await supabase
       .from('orders')
       .select(`
         *,
         clients (
           *
         )
       `)

     if (data) {
       console.log(data)
       orders.value = data
       await incrementViews()
     }
   } catch (error) {
     console.log(error)
   }
 }

 const insertOrder = async () => {
   try {
     const { data, error } = await supabase
       .from('orders')
       .insert(
         {
           address: '123 Main St',
           zip_code: '10001',
           city: 'New York',
           name: 'Emily Williams',
           client_id: 'afe8c2a3-3d38-4c0b-afca-23ce643d50f9',
           price: 12
         }
       )

     if (data) {
       console.log(data)
     }
   } catch (error) {
     console.log(error)
   }
 }

 const fetchOrdersFromEmilyWilliams = async () => {
   try {
     const { data: orders, error } = await supabase
       .from('orders')
       .select('*')
       .eq('name', 'Emily Williams')
      .insert(
        {
          address: '123 Main St',
          zip_code: '10001',
          city: 'New York',
          name: 'Emily Williams',
          client_id: 'afe8c2a3-3d38-4c0b-afca-23ce643d50f9',
          price: 12
        }
      )

     if (orders) {
       console.log(orders)
     }
   } catch (error) {
     console.log(error)
   }
 }

 const updateEmilyWilliamOrder = async () => {
   try {
     const { data, error } = await supabase
       .from('orders')
       .update({ price: '200' })
       .eq('id', "303f6f57-0929-4d91-b0b8-1a3f0513e993")

     if (data) {
       console.log(data)
     }
   } catch (error) {
     console.log(error)
   }
 }

 const deleteEmilyWilliamOrder = async () => {
   try {
     const { data, error } = await supabase
       .from('orders')
       .delete()
       .eq('id', "303f6f57-0929-4d91-b0b8-1a3f0513e993")

     if (data) {
       console.log(data)
     }
   } catch (error) {
     console.log(error)
   }
 }

 const channel = supabase
   .channel('my_new_channel_for_order')
   .on(
     'postgres_changes',
     {
       event: '*',
       schema: 'public',
       table: 'orders'
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
     }
   )
   .subscribe()



 const incrementViews = async () => {
   try {
     const { data, error } = await supabase.rpc('increment', {
       row_id: 'e1f3321e-3949-4ec1-afb8-65cbe3846648'
     })

     if (data) {
       console.log("success")
       console.log(data)
     }

   } catch (error) {
     console.log(error)
   }
 }

 fetchOrders(); */

-->

<!-- <template>
  <main class="flex items-center justify-between">
    <div class="container px-2 mx-auto my-8">
      <User />
      <div v-for="order, index in orders" :key="index">
        <div v-if="order" class="grid grid-cols-5 gap-3 px-3 py-2 my-2 border rounded-lg shadow-md">
          <div v-if="order.name">{{ order.name }}</div>
          <div>${{ order.price || 0 }}</div>
          <div v-if="order.address">{{ order.address }}</div>
          <div v-if="order.zip_code">{{ order.zip_code }}</div>
          <div v-if="order.city">{{ order.city }}</div>
        </div>
      </div>
    </div>
  </main>
</template> -->
