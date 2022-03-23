<template>
    <div>
        <h3>Account Information</h3>
        <form @submit.prevent="infoSubmit">
            <div class="mb-3">
                <label>First Name</label>
                <input v-model="infoData.first_name" class="form-control" name="first_name">
            </div>
            <div class="mb-3">
                <label>Last Name</label>
                <input v-model="infoData.last_name" class="form-control" name="last_name">
            </div>
            <div class="mb-3">
                <label>Email</label>
                <input v-model="infoData.email" class="form-control" name="email">
            </div>
            <button class="btn btn-outline-secondary">Save</button>
        </form>

        <hr class="mt-4">
        <h3>Change Password</h3>
        <form @submit.prevent="passwordSubmit">
            <div class="mb-3">
                <label>Password</label>
                <input v-model="passwordData.password" class="form-control" name="password" type="password">
            </div>
            <div class="mb-3">
                <label>Password Confirm</label>
                <input v-model="passwordData.password_confirm" class="form-control" name="password_confirm" type="password">
            </div>
            <button class="btn btn-outline-secondary">Save</button>
        </form>
    </div>
</template>

<script>
import {computed, reactive, watch} from "vue";
import {useStore} from "vuex";
import axios from "axios";

export default {
    name: "Profile",
    setup() {
        const store = useStore();
        const user = computed(() => store.state.user);

        const infoData = reactive({
            first_name: user.value.first_name,
            last_name: user.value.last_name,
            email: user.value.email
        });
        const passwordData = reactive({
            password: '',
            password_confirm: ''
        });

        watch(user, () => {
            infoData.first_name = user.value.first_name;
            infoData.last_name = user.value.last_name;
            infoData.email = user.value.email;
        });

        const infoSubmit = async () => {
            const {data} = await axios.put('users/info', infoData);
            await store.dispatch('setUser', data);
        }
        const passwordSubmit = async () => {
            await axios.put('users/password', passwordData);
        }

        return {
            infoData,
            passwordData,
            infoSubmit,
            passwordSubmit
        }
    }
}
</script>

<style scoped>

</style>
