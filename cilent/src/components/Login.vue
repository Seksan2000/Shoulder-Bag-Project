<template>
<center>
  <div class="container">
    <center>
    <h1>User Login</h1>
    <form v-on:submit.prevent="onLogin">
      <p>Username: <input class="form-control" type="text" v-model="email" /></p>
      <p>Password: <input class="form-control" type="password" v-model="password" /></p>
      <p><button class="btn btn-outline-dark" type="submit">Login</button> 
      <button class="btn btn-outline-dark" v-on:click="navigateTo('/user/create')">Create User </button></p>
      <div class="error" v-if="error">{{error}}</div>
      
    </form>
    </center>
  </div>
</center>
</template>
<script>
import AuthenService from "@/services/AuthenService";
export default {
  data() {
    return {
      email: "",
      password: "",
      error: null
    };
  },
  methods: {
      navigateTo(route){
            this.$router.push(route)
        },
    async onLogin() {
      try {
        const response = await AuthenService.login({
          email: this.email,
          password: this.password,
        });
        this.$store.dispatch('setToken', response.data.token)
        this.$store.dispatch('setUser', response.data.user)
        this.$router.push({
          name: 'home'
        })
      } catch (error) {
        console.log(error);
        this.error = error.response.data.error
        this.email = ''
        this.password = ''
      }
    },
  }
};
</script>
<style scoped>
  .error{
    color:red;
  }
  div {
    margin-left: 21%;
    margin-top: 15%;
}
/* Style inputs with type="text", select elements and textareas */
p[type=text], select, textarea {
  width: 100%; /* Full width */
  padding: 12px; /* Some padding */ 
  border: 1px solid #ccc; /* Gray border */
  border-radius: 4px; /* Rounded borders */
  box-sizing: border-box; /* Make sure that padding and width stays in place */
  margin-top: 6px; /* Add a top margin */
  margin-bottom: 16px; /* Bottom margin */
  resize: vertical /* Allow the user to vertically resize the textarea (not horizontally) */
}
/* Style the submit button with a specific background color etc */
p[type=submit] {
  background-color: #04AA6D;
  color: FDF5E6;
  padding: 12px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
/* When moving the mouse over the submit button, add a darker green color */
p[type=submit]:hover {
  background-color: #73C2FB;
}
/* Add a background color and some padding around the form */
.container {
  border-radius: 20px;
  background: #1134A6; /* For browsers that do not support gradients */
  background: -webkit-linear-gradient(left, #1134A6 , #95C8D8); /* For Safari 5.1 to 6.0 */
  background: -o-linear-gradient(right, #1134A6, #95C8D8); /* For Opera 11.1 to 12.0 */
  background: -moz-linear-gradient(right, #1134A6, #95C8D8); /* For Firefox 3.6 to 15 */
  background: linear-gradient(to right, #1134A6 , #95C8D8); /* Standard syntax */
  padding: 20px;
}
input {
  width: 450px;
}
</style>