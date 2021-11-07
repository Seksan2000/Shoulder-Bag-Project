<template>
<div class="row1">
  <div class="col-75">
    <div class="container">
      <form v-on:submit.prevent = "editeUser">

        <div class="row">
          <div class="col-50">
            <h3>Edite User</h3>
                <hr>

                <div class="row">
                    <div class="col">
                        <label for="fname"><i class="fa fa-user"></i> Name
                        <input type="text" v-model="user.name" placeholder="Name"></label>
                    </div>
                <div class="col">
                        <label for="lname"> Lastname
                        <input type="text" v-model="user.lastname" placeholder="Lastname"></label>
                    </div>
                </div>
                <div>
                    <label for="email"><i class="fa fa-envelope"></i> Email 
                        <input type="text" v-model="user.email" placeholder="Email"></label>
                    <label for="pass"><i class="fa fa-lock"></i> Password 
                        <input type="text" v-model="user.password" placeholder="Password"></label>
                </div>
          </div>
        </div>
    <hr>
    <div>
        <p>name: {{user.name}}</p>
        <p>lastname: {{user.lastname}}</p>
        <p>email: {{user.email}}</p>
        <p>password: {{user.password}}</p>
    </div>
    <div>
        <p><button class="btn btn-outline-dark" type="submit">Edite user</button>
                    <button type="button" class="btn btn-outline-dark" v-on:click="navigateTo('/users')">Back</button></p> </div>
      </form>
    </div>
  </div>
</div>
</template>
<script>
import UsersService from '@/services/UsersService'
export default {
    data() {
        return {
            user: {
                name: '',
                lastname: '',
                email: '',
                password: '',
                status: 'active'
            }
        }
    },
    methods: {
        navigateTo(route) {
      console.log(route);
      this.$router.push(route);
    },
        async editUser() {
            try {
                await UsersService.put(this.user)
                this.$router.push({
                    name: 'users'
                })
            } catch (error) {
                console.log(error)
            }
        }
    },
    async created() {
        try {
           let userId = this.$route.params.userId
           this.user = (await UsersService.show(userId)).data 
        } catch (error) {
            console.log (error)
        }
    }
}
</script>
<style scoped>
div {
    margin-left: 2%;
    margin-top: 2%;
}
.row1 {
    margin-left: 10%;
}
.row {
  display: -ms-flexbox; /* IE10 */
  display: flex;
  -ms-flex-wrap: wrap; /* IE10 */
  flex-wrap: wrap;
  margin: 0 -16px;
}
.col-25 {
  -ms-flex: 25%; /* IE10 */
  flex: 25%;
}
.col-50 {
  -ms-flex: 50%; /* IE10 */
  flex: 50%;
}
.col-75 {
  -ms-flex: 75%; /* IE10 */
  flex: 75%;
}
.col-25,
.col-50,
.col-75 {
  padding: 0 16px;
}
.container {
  background-color: #f2f2f2;
  padding: 5px 20px 15px 20px;
  border: 1px solid lightgrey;
  border-radius: 3px;
}
input[type=text] {
  width: 100%;
  margin-bottom: 20px;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 3px;
}
label {
  margin-bottom: 10px;
  display: block;
}
.icon-container {
  margin-bottom: 20px;
  padding: 7px 0;
  font-size: 24px;
}
span.price {
  float: right;
  color: grey;
}
/* Responsive layout - when the screen is less than 800px wide, make the two columns stack on top of each other instead of next to each other (and change the direction - make the "cart" column go on top) */
@media (max-width: 800px) {
  .row {
    flex-direction: column-reverse;
  }
  .col-25 {
    margin-bottom: 20px;
  }
}
</style>