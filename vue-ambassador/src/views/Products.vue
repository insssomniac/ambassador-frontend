<template>
    <div class="alert alert-info" v-if="link">
        {{ link }}
    </div>
    <div class="alert alert-danger" v-if="error">
        {{ error }}
    </div>
    <div class="row mb-4">
        <div class="col-md-10 input-group">
            <input class="form-control" placeholder="Search" @keyup="search($event.target.value)"/>
            <div class="input-group-append">
                <select class="form-select" @change="sort($event.target.value)">
                    <option>Select</option>
                    <option value="asc">Price Ascending</option>
                    <option value="desc">Price Descending</option>
                </select>
            </div>
        </div>
        <div class="col-md-2" v-if="selected.length > 0">
            <button class="btn btn-info" @click="generateLink">Generate Link</button>
        </div>
    </div>

    <div class="row">
        <div class="col-md-4" v-for="product in products" :key="product.id" @click="select(product.id)">
            <div class="card mb-4 shadow-sm" :class="selected.some(s => s === product.id) ? 'selected' : ''">
                <img :src="product.image" height="200" alt="">
                <div class="card-body">
                    <p class="card-text">{{ product.title }}</p>
                    <div class="d-flex justify-content-between align-items-center">
                        <small class="text-muted">${{ product.price }}</small>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div class="d-flex justify-content-center mt-4" v-if="filters.page < lastPage">
        <button class="btn btn-primary" @click="loadMore">Load More</button>
    </div>
</template>

<script lang="ts">
import {ref, SetupContext} from "vue";
import axios from "axios";

export default {
    name: "Products",
    props: ['products', 'filters', 'lastPage'],
    emits: ['set-filters'],
    setup(props: any, context: SetupContext) {
        const selected = ref<number[]>([]);
        const link = ref<string>('');
        const error = ref<string>('');

        const search = (s: string) => {
            context.emit('set-filters', {
                ...props.filters,
                s,
                page: 1
            })
        }

        const sort = (sort: string) => {
            context.emit('set-filters', {
                ...props.filters,
                sort,
                page: 1
            });
        }

        const loadMore = () => {
            context.emit('set-filters', {
                ...props.filters,
                page: props.filters.page + 1
            });
        }

        const select = (id: number) => {
            if (selected.value.some(s => s === id)) {
                selected.value = selected.value.filter(s => s !== id);
                return;
            }

            selected.value = [...selected.value, id];
        }

        const generateLink = async () => {
            try {
                const {data} = await axios.post('links', {
                    products: selected.value
                });

                link.value = `Link generated: ${process.env.VUE_APP_CHECKOUT_URL}/${data.code}`;
            } catch (e) {
                error.value = 'You should be logged in to generate a link!';
            } finally {
                setTimeout( () => {
                    link.value = '';
                    error.value = '';
                }, 5000);
            }

        }

        return {
            selected,
            link,
            error,
            search,
            sort,
            loadMore,
            select,
            generateLink
        }
    }
}
</script>

<style scoped>
    .card {
        cursor: pointer;
    }

    .card.selected {
        border: 4px solid darkcyan;
    }
</style>
