<template>
    <div>
      <form @submit.prevent="login">
        <input type="email" v-model="email" placeholder="Email">
        <input type="password" v-model="password" placeholder="Password">
        <button type="submit">Login</button>
      </form>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import Cookies from 'js-cookie';
  
  export default {
    data() {
      return {
        email: '',
        password: ''
      };
    },
    methods: {
      async login() {
        try {
          const response = await axios.post('http://127.0.0.1:8000/api/login', {
            email: this.email,
            password: this.password
          });
  
          if (response.data.success) {
          // Simpan token ke dalam cookie
          Cookies.set('token', response.data.token);
          // Redirect ke halaman dashboard
          this.$router.push('/dashboard');
        } else {
          // Tampilkan pesan kesalahan
          alert(response.data.message);
        }
        } catch (error) {
          console.error('Error:', error);
          // Handle error
        }
      }
    }
  };
  </script>
  