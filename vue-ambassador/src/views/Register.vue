<template>
    <section class="text-center register-section">
        <form @submit.prevent="submit" class="form-signin">
            <h1 class="h3 mb-3 font-weight-normal">Create a new account</h1>
            <input v-model="first_name" type="text" class="form-control mb-3" placeholder="First name" required>
            <input v-model="last_name" type="text" class="form-control mb-3" placeholder="Last name" required>
            <input v-model="email" type="email" class="form-control mb-3" placeholder="Email address" required>
            <input v-model="password" type="password" class="form-control mb-3" placeholder="Password" required>
            <input v-model="password_confirm" type="password" class="form-control" placeholder="Password Confirm" required>
            <button class="btn btn-lg btn-primary btn-block sign-in-btn" type="submit">Submit</button>
            <p class="mt-5 mb-3 text-muted">&copy; 2022</p>
        </form>
    </section>
</template>

<script>
import axios from "axios";

export default {
    name: "Register",
    data () {
        return {
            first_name: '',
            last_name: '',
            email: '',
            password: '',
            password_confirm: ''
        }
    },
    methods: {
        async submit() {
            await axios.post('register', {
                first_name: this.first_name,
                last_name: this.last_name,
                email: this.email,
                password: this.password,
                password_confirm: this.password_confirm
            });

            await this.$router.push('/login');
        }
    }
}
</script>

<style scoped>
.register-section {
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    display: flex;
}

.form-signin {
    width: 100%;
    max-width: 330px;
    padding: 15px;
    margin: auto;
}
.form-signin .checkbox {
    font-weight: 400;
}
.form-signin .form-control {
    position: relative;
    box-sizing: border-box;
    height: auto;
    padding: 10px;
    font-size: 16px;
}
.form-signin .form-control:focus {
    z-index: 2;
}
.form-signin input[type="email"] {
    margin-bottom: -1px;
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 0;
}
.form-signin input[type="password"] {
    margin-bottom: 10px;
    border-top-left-radius: 0;
    border-top-right-radius: 0;
}
.sign-in-btn{
    margin-top: 20px;
}
</style>
