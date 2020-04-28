<template>
  <div class="dashboard">
    <h1>Dashboard</h1>
    <p>You must be authenticated to see this. You are logged in as {{user.sub}}.</p>
    
  </div>
</template>

<script>
import auth from '@/auth.js'

export default {
  name: 'dashboard',
  data() {
    return {
      user: auth.getUser()
    }
  },
  methods: {
   
  }
}
</script>
