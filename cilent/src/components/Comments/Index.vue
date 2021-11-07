<template>
    <div class="product">
        <h2>All Product</h2><hr>
        <div class="welcome-text">
                <center><img src="@/assets/1.jpg"></center>
                <h2></h2>
                <h5></h5>
        <hr />
                <center><img src="@/assets/2.jpg"></center>
                <h2></h2>  
        <hr />
                <center><img src="@/assets/3.jpg"></center>
                <h2></h2>
                <h5> </h5>
        <hr />
        </div>
        <b-button-group>
            <b-button variant="dark" v-on:click="navigateTo('/blogs')">Back</b-button>
        </b-button-group>
    </div>
</template>
<script>

export default {
data () {
    
},
methods: {
    navigateTo (route) {
        this.$router.push(route)
    },
}
}
</script>
<style scoped>
    img {
        width: 500px;
        height: 500px;
    }

	h2 {
		font-size: 30px;
		font-family: 'robotobold';
		color: rgb(117, 117, 117);
		margin-bottom: 30px;
	}

	h5 {
		font-size: 22px;
		color: rgb(117, 117, 117);
	}

</style>