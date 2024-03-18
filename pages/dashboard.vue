<template>
    <div>
      <h1>Welcome to Dashboard</h1>
      <p>Name: {{ name }}</p>
      <p>Email: {{ email }}</p>
      <p>Token: {{ token }}</p>
      <button @click="logout">Logout</button>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  import Cookies from 'js-cookie';
  
  export default {
    data() {
      return {
        name: '',
        email: '',
        token:''
      };
    },
    async mounted() {
      await this.fetchUserData();
    },
    methods: {
      async fetchUserData() {
        try {
          const token = Cookies.get('token');
          const response = await axios.get('http://127.0.0.1:8000/api/dashboard', {
            headers: {
              Authorization: `Bearer ${token}`
            }
          });
          
          if (response.data.status === 'Token is Expired') {
          // Token expired, logout user
          this.logout();
        } else {
          this.name = response.data.name;
          this.email = response.data.email;
          this.token = token;
        }


        } catch (error) {
          console.error('Error:', error);
        }
      },
      async logout() {
        try {
          const token = Cookies.get('token');
          await axios.get('http://127.0.0.1:8000/api/logout', {
            headers: {
              Authorization: `Bearer ${token}`
            }
          });
          // Hapus token dari cookie
          Cookies.remove('token');
          // Redirect ke halaman login
          this.$router.push('/');
        } catch (error) {
          console.error('Error:', error);
        }
      }
    }
  };
  </script>
  