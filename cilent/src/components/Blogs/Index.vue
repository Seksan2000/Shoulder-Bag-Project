<template>
    <div>
        <br>
        <h2> รายการสั่งซื้อสินค้าทั้งหมด </h2><hr>

        <!-- <p><button v-on:click="logout"> Logout </button></p> -->

        <h4> จำนวนOrder : {{blogs.length}}</h4>
        <p><b-button variant="success" v-on:click="navigateTo('/blog/create')"> สั่งซื้อสินค้าและชำระ </b-button></p>
        <div v-for="blog in blogs" v-bind:key="blog.id">
            <p>id : {{ blog.id }} </p>
            <p>ชื่อ-นามสกุล : {{ blog.title }} </p>
            <p>ที่อยู่และเบอร์โทรติดต่อ : {{ blog.content }} </p>
            <p>สีและขนาด : {{ blog.category }} </p>
            <p>จำนวน : {{ blog.status }} </p>
            <b-button-group>
                <b-button v-on:click="navigateTo('/blog/'+ blog.id)"> ดูOrder </b-button>
                <b-button v-on:click="navigateTo('/blog/edit/'+ blog.id)"> แก้ไขOrder </b-button>
                <b-button v-on:click="deleteBlog(blog)"> ลบข้อมูล </b-button>
            </b-button-group>
            <hr>
        </div>
    </div>
</template>
<script>
import BlogsService from '@/services/BlogsService'
export default {
    data () {
        return { 
            blogs: []
        }
    },
    async created () {
        this.blogs = (await BlogsService.index()).data
    },
    methods: {
       /*  logout () {
            this.$store.dispatch('setToken', null)
            this.$store.dispatch('setBlog', null)
            this.$router.push({
                name: 'login'
            })
        }, */
        navigateTo (route) {
            this.$router.push(route)
        },
        async deleteBlog (blog) {
            let result = confirm("Want to delete?")
            if (result) {
                try {
                    await BlogsService.delete(blog)
                    this.refreshData()
                } catch (err) {
                    console.log(err)
                }
            }
        },
        async refreshData() {
            this.blogs = (await BlogsService.index()).data
        }
    }
}
</script>
<style scoped>
</style>