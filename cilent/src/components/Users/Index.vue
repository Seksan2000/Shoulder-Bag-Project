<template>
<div class="users">
    <h1>Users</h1>
    <hr/><div v-if="users.length">
        <h4>จำนวนพนักงาน : {{users.length}}</h4>
        <b-button-group>
            <b-button variant="primary" v-on:click="navigateTo('/user/create')">สร้างผู้ใช้งาน</b-button>
            <b-button variant="danger" v-on:click="logout">Logout</b-button>
        </b-button-group>
        <div v-for="user in users" v-bind:key="user.id">
            <p>ID : {{user.id}}</p>
            <p>ชื่อ-นามสกุล : {{user.name}} - {{user.lastname}}</p>
            <p>Email : {{user.email}}</p>
            <p>Password : {{user.password}}</p>
            <b-button-group>
                <b-button v-on:click="navigateTo('/user/'+user.id)">ดูข้อมูลผู้ใช้</b-button>
                <b-button v-on:click="navigateTo('/user/edit/'+user.id)">แก้ไขข้อมูล</b-button>
                <b-button v-on:click="deleteUser(user)">ลบข้อมูล</b-button>
            </b-button-group>
            <hr>
        </div>
    </div>
</div>
</template>
<script>
import UsersService from '@/services/UsersService';
export default {
    data(){
        return {
            users:[]
        }
    },
    async created() {
        try {
            this.users = (await UsersService.index()).data;
        } catch (error) {
            console.log(error);
        }
    },
    methods:{
        navigateTo(route){
            this.$router.push(route)
        },
        async deleteUser(user){
            let result = confirm("Want to delete")
            if(result){
                try{
                    await UsersService.delete(user)
                    this.refreshData()
                } catch(error){
                    console.log(error)
                }
            }
        },
        async refreshData () {
            this.users = (await UsersService.index()).data
        },
        logout(){
             this.$store.dispatch('setToken',null)
             this.$store.dispatch('setUser',null)
             this.$router.push({
                 name: 'login'
             })
         }
    },
};
</script>
<style scoped>
    
</style>