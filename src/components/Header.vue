<script setup>
import { supabase } from "@/supabase"
import { useAuth } from "@/composables/useAuth"

/* const { router } = useAuth()

const signOut = async () => {
  const { error } = await supabase.auth.signOut();
  if (error) {
    console.log('Sign-out error:', error.message);
  } else {
    router.push('/');
  }
}; */

const { setUser } = useAuth();

const signOut = async () => {
  const { error } = await supabase.auth.signOut();
  if (error) {
    console.log('Sign-out error:', error.message);
  } else {
    setUser(null); // Update the user state explicitly
    router.push('/');
  }
};


</script>

<template>
  <header class="py-4 bg-gray-900">
    <div class="container flex items-center justify-between mx-auto">
      <h1 class="text-lg font-bold text-white">My Nuxt Supabase app</h1>
      <button @click="signOut" class="w-auto px-4 py-2 font-semibold text-white bg-red-600 rounded hover:bg-red-700" >
        Signout
      </button>
    </div>
  </header>
</template>