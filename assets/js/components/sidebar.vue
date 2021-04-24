<template>
    <div
        :class="[this.$style.component, 'p-3', 'mb-5']"
    >
        <div v-show="!collapsed">
            <h5 class="text-center">
                Categories
            </h5>
            <Loading v-show="loading"/>
            <ul class="nav flex-column mb4">
                <li class="nav-item">
                    <a
                        :class="{
                            'nav-link': true,
                            'selected': currentCategoryId ===null,
                        }"
                        href="/"
                    >All Products</a>
                </li>
                <li
                    v-for="category in categories"
                    :key="category['@id']"
                    class="nav-item"
                >
                    <a
                        :class="{
                            'nav-link': true,
                            'selected': category['@id'] === currentCategoryId,
                        }"
                        :href="`/category/+${category.id}`"
                    >{{ category.name}}</a>
                </li>
            </ul>
            <hr>
        </div>
        <div class="d-flex justify-content-end">
            <button
                class="btn btn-secondary btn-sm"
                @click="$emit('toggle-collapsed')"
                v-text="collapsed ? '>>' : '<< Collapse'"
            >
            </button>
        </div>

    </div>
</template>

<script>
import { fetchCategories } from '../services/categories-service';
import Loading from './loading';

export default {
    name: 'Sidebar',
    components: {
        Loading,
    },
    props: {
        collapsed: {
            type: Boolean,
            required: true,
        },
        currentCategoryId: {
            type: String,
            default: null,
        },
        categories: {
            type: Array,
            required: true,
        },
    },
    computed: {
        loading() {
            return this.categories.length === 0;
        },
    },

};
</script>

<style lang="scss" module>
@import '../../scss/components/light-component';
.component {
  @include light-component;
  ul {
    li a:hover {
      background: $blue-component-link-hover;
    }
    :global li a.selected{
      background: $light-component-border;
    }
  }
}


</style>
