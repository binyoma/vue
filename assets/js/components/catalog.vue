<template>
    <div>
        <div class="row">
            <div class="col-3">
                <TitleComponent
                    :current-category-id="currentCategoryId"
                    :categories="categories"
                />
            </div>
            <div class="col-9">
                <search-bar
                    @search-products="onSearchProducts"
                />
            </div>
        </div>
        <div class="row">
            <Product-list
                :products="products"
                :loading="loading"
            />
        </div>
        <div class="row">
            <legend-component  :title="legend"/>
        </div>
    </div>
</template>

<script>
import { fetchProducts } from '../services/products-service';
import LegendComponent from './legend';
import ProductList from './product-list/index';
import TitleComponent from './title';
import SearchBar from './search-bar';


export default {
    name: 'Catalog',
    components: {
        SearchBar,
        LegendComponent,
        ProductList,
        TitleComponent,
    },
    props: {
        currentCategoryId: {
            type: String,
            default: null,
        },
        categories: {
            type: Array,
            required: true,
        },
    },
    data() {
        return {
            products: [],
            loading: false,
            legend: 'Shipping takes 10-12 weeks, and products probably won\'t work',
            searchTerm: null,
        };
    },
    watch: {
        currentCategoryId() {
            this.loadProducts();
        },
    },
    created() {
        this.loadProducts();
    },
    methods: {
        /**
        * Handles a change in the searchTerm provided by the search bar and fetch new products
        * @param {string}term
        */
        onSearchProducts({ term }) {
            this.searchTerm = term;
            this.loadProducts();
        },
        async loadProducts() {
            this.loading = true;
            let response;
            try {
                response = await fetchProducts(this.currentCategoryId, this.searchTerm);
                this.loading = false;
            } catch (e) {
                this.loading = false;
                return;
            }
            this.products = response.data['hydra:member'];
        },
    },
};
</script>
