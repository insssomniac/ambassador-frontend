<template>
    <div class="d-flex flex-column flex-md-row align-items-center p-3 px-md-4 mb-3 bg-white border-bottom shadow-sm">
        <nav class="my-0 mr-md-auto font-weight-normal">
            <a class="p-2 text-dark" href="#">Frontend</a>
            <a class="p-2 text-dark" href="#">Backend</a>
        </nav>

        <div v-if="user">
            <router-link to="/profile" class="btn"><b class="text-muted">{{ user.first_name }}&nbsp;{{ user.last_name }}</b></router-link>
            <a class="btn btn-outline-primary mr-2" href="#" @click.prevent="logout">Logout</a>
        </div>

        <div v-if="!user">
            <router-link to="/register" class="btn btn-primary mr-2">Sign up</router-link>
            <router-link to="/login" class="btn btn-outline-primary">Login</router-link>
        </div>

    </div>
</template>

<script>
import {useStore} from "vuex";
import {computed} from "vue";
import axios from "axios";

export default {
    name: "Nav",
    setup() {
        const store = useStore();
        const user = computed(() => store.state.user);

        const logout = async () => {
            await axios.post('logout');
            await store.dispatch('setUser', null);
        }

        return {
            user,
            logout
        }
    }
}
</script>

<style scoped>

</style>
