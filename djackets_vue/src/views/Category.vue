<template>
    <div class="page_category">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h2 class="is-size-2 has-text-centered">{{ category.name }}</h2>
            </div>
        </div>

        <div class="columns is-multiline">

            <ProductBox
                v-for="product in category.products"
                v-bind:key="product.id"
                v-bind:product="product" />
	    </div>
    </div>
</template>


<script>
import axios from 'axios'
import { toast } from 'bulma-toast'
import ProductBox from '@/components/ProductBox.vue'

export default {
    name: 'Category',
    components: {
        ProductBox,
    },
    data() {
        return {
            category: {
                products: []
            }
        }
    },
    mounted() {
        this.getCategory()
    },
    watch: {
        $route() {
            this.getCategory();
        }
    },
    methods: {
        async getCategory() {
            const categorySlug = this.$route.params.category_slug

            this.$store.commit('setIsloading', true)

            axios
                .get(`/api/v1/products/${categorySlug}/`)
                .then(response => {
                    this.category = response.data
                    document.title = this.category.name + ' | Djackets'
                })
                .catch(error => {
                    console.log(error)

                    toast({
                        message: 'Something went wrong. Plaese try again.',
                        type: 'is-danger',
                        dismissible: true,
                        pauseOnHover: true,
                        duration: 2000,
                        position: 'bottom-right',
                    })
                })

            this.$store.commit('setIsloading', false)
        }
    }
}

</script>