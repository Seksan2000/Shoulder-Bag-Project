<template>
  <div>
    <div>
      <b-navbar type="dark" variant="info">
        <b-navbar-nav>
          <b-nav-item router-link :to="{ name: 'home' }">หน้าแรก</b-nav-item>
          <b-nav-item router-link :to="{ name: 'blogs' }"> รายการที่สั่ง </b-nav-item>
          <b-nav-item-dropdown text="User" right >
            <b-dropdown-item router-link :to="{ name: 'users' }"> Users </b-dropdown-item>
            <b-dropdown-item router-link :to="{ name: 'login' }"> Login </b-dropdown-item>
            <b-dropdown-item a v-on:click.prevent="logout" href="#"> Logout </b-dropdown-item>
          </b-nav-item-dropdown>
        <div class="clearfix"></div>
        </b-navbar-nav>
      </b-navbar>
    </div>
  </div>
</template>
<script>
export default {
  methods:{
  logout () {
          this.$store.dispatch('setToken', null)
          this.$store.dispatch('setUser', null)
          this.$router.push({
            name: 'login'
    })
    }
  }
}
</script>
<style scoped>

.clearfix {
  clear: left;
}
</style>