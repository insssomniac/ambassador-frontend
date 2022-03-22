<template>
    <nav class="navbar navbar-dark fixed-top bg-dark flex-md-nowrap p-0 shadow">
        <a class="navbar-brand col-sm-3 col-md-2 mr-0" href="#">Company name</a>

        <nav class="my-2 my-md-0 mr-md-3">
            <router-link to="/profile" class="p-2 text-white text-decoration-none">
                {{user.first_name}}&nbsp;{{user.last_name}}
            </router-link>
            <a href="#" class="p-2 text-white text-decoration-none" @click="logout">Sign Out</a>
        </nav>
    </nav>
</template>

<script>
import axios from "axios";

export default {
    name: "Nav",
    computed: {
        user() {
            return this.$store.state.user;
        }
    },
    methods: {
        async logout() {
            await axios.post('logout');
            await this.$router.push('/login');
        }
    }
}
</script>

<style scoped>

</style>
