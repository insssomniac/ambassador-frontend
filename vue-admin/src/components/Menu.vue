<template>
    <nav class="col-md-2 d-none d-md-block bg-light sidebar">
        <div class="sidebar-sticky">
            <ul class="nav flex-column">
                <li class="nav-item">
                    <router-link to="/users" class="nav-link" active-class="active">Users
                    </router-link>
                </li>
                <li class="nav-item">
                    <router-link to="/products" class="nav-link" active-class="active">Products
                    </router-link>
                </li>
                <li class="nav-item">
                    <router-link to="/orders" class="nav-link" active-class="active">Orders
                    </router-link>
                </li>
            </ul>
        </div>
    </nav>
</template>

<script>
export default {
    name: "Menu"
}
</script>

<style scoped>

</style>
