<template>
    <div>
        <section v-if="loading">
        </section>
        <section v-else>
            <div class="info-block">
                <img class="product-image" :src="product.images[0].main">
            </div>
            <div class="info-block">
                <div class="statuses">
                    <div class="status ship-status">
                        <template v-if="shipping.props.ready_to_ship">
                            <span>Ready to Ship</span>
                        </template>
                        <template v-else>
                            <span>Not ready to ship</span>
                        </template>
                    </div>    
                    <div class="status stock-status">
                        <template v-if="shipping.props.in_stock">
                            <img src="https://img.icons8.com/material-outlined/12/000000/ok--v1.png"/>
                            <span> In stock</span>
                        </template>
                        <template v-else>
                            <img src="https://img.icons8.com/material-outlined/12/000000/ok--v1.png"/>
                            <span> Out of stock</span>
                        </template>
                    </div>
                    <div class="status dispatch-status">
                        <template v-if="shipping.props.fast_dispatch">
                            <img src="https://img.icons8.com/material-outlined/12/000000/ok--v1.png"/>
                            <span> Fast Dispatch</span>
                        </template>
                        <template v-else>
                            <img src="https://img.icons8.com/material-outlined/12/000000/ok--v1.png"/>
                            <span> No Fast Dispatch</span>
                        </template>
                    </div>
                </div>
                <div>
                    <span class="product-name">{{product.name}}</span>
                    <span class="product-tag" v-for="tag in product.tags" :key="tag.key">{{tag}}</span>
                </div>
                <StarRating :current="reviews.rating" :countReviews="reviews.count"/>
            </div>
            <div class="info-block"></div>
        </section>
    </div>
</template>

<script>
import axios from 'axios';
import StarRating from './other/StarRating.vue'

export default {
    name:"ProductCard",
    components: {
        StarRating
    },
    data() {
        return {
            loading: true,
            errored: false,
            product: {
                images: [], //carousel? there is an array
                name: '', 
                tags: []

            },
            shipping: {
                props: {
                    fast_dispatch: null,
                    in_stock: null,
                    ready_to_ship: null
                }
            },
            reviews: {
                rating: null,
                count: null,
                total_buyers: null
            }
        }
    },
    mounted () {
        axios
        .get('https://fe-assignment.vaimo.net/')
        .then(response => {
            console.log(response.data);
            this.product.name = response.data.product.name;
            this.product.tags = response.data.product.tags;
            this.product.images = response.data.product.gallery;

            this.shipping.props.fast_dispatch = response.data.product.shipping.props.fast_dispatch;
            this.shipping.props.in_stock = response.data.product.shipping.props.in_stock;
            this.shipping.props.ready_to_ship = response.data.product.shipping.props.ready_to_ship;

            this.reviews.rating = response.data.product.reviews.rating;
            this.reviews.count = response.data.product.reviews.count;
            this.reviews.total_buyers = response.data.product.reviews.total_buyers;
        })
        .catch(error => {
            console.log(error)
            this.errored = true
        })
        .finally(() => this.loading = false)
    }


}
</script>

<style lang="scss" scoped>

    .info-block{
        display: inline-block;
    }
    .active{
        background: tomato;
        height: 50px;
        width: 50px;
    }

    .statuses{
        display:flex;    
        align-items:flex-start;
    }

    .status{
        display: inline-block;
        border-radius: 0 2px 2px 0;
        height: 20px;
    }

    .product-image{
        width: 416px;
        height: 416px;
    }

    .product-name{
        color: #333333;
        font-family: Roboto;
        font-size: 16px;
        line-height: 24px;
        width: 526px;
        text-align: left;
    }

    .product-tag{
        color: #999999;
        font-family: Roboto;
        font-size: 12px;
        line-height: 16px;
        text-align: left;
        padding-top: 2px;
        padding-right: 8px;
        padding-bottom: 2px;
        padding-left: 8px;
    }
    

</style>