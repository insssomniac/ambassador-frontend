<template>
    <v-app>
        <Nav/>

        <div class="container-fluid">
            <div class="row">
                <Menu/>

                <main role="main" class="col-md-9 ml-sm-auto col-lg-10 px-4">
                    <router-view />
                </main>
            </div>
        </div>
    </v-app>
</template>

<script>
import Nav from "../components/Nav";
import Menu from "../components/Menu";
import axios from "axios";
import {User} from "@/models/user";

export default {
    name: "Layout",
    components: {Menu, Nav},
    async mounted() {
        try {
            const {data} = await axios.get('user');
            await this.$store.dispatch('setUser', data)
        } catch (e) {
            await this.$router.push('/login');
        }
    }
}
</script>

<style scoped>

</style>
