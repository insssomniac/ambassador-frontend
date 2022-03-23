<template>
    <section class="text-center">
        <div class="container">
            <h1 class="jumbotron-heading">{{ title }}</h1>
            <p class="lead text-muted">{{ description }}</p>
            <p v-if="!user">
                <router-link to="/register" class="btn btn-primary my-2 mr-4">Sign up</router-link>
                <router-link to="/login" class="btn btn-secondary my-2">Login</router-link>
            </p>
        </div>
    </section>
</template>

<script>
import {computed, ref, watch} from "vue";
import {useStore} from "vuex";

export default {
    name: "Header",
    setup() {
        const title = ref('');
        const description = ref('')
        const store = useStore();
        const user = computed(() => store.state.user);

        title.value = user.value ? '$' + user.value.revenue : 'Welcome';
        description.value = user.value ? 'You have earned this far' : 'Share links to earn money';

        watch(user, () => {
            title.value = user.value ? '$' + user.value.revenue : 'Welcome';
            description.value = user.value ? 'You have earned this far' : 'Share links to earn money';
        });

        return {
            title,
            description,
            user
        }
    }
}
</script>

<style scoped>

</style>
