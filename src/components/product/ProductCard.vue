<template>
    <div>
        <section v-if="loading">
        </section>
        <section id="main" v-else>
            <div class="info-block-1">
                <img class="product-image" :src="product.images[0].main" width="416px" height="416px">
            </div>
            <div class="info-block-2">
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
                            <img src="/assets/icons8-ok.png" height="12px" width="12px">
                            <span> In stock</span>
                        </template>
                        <template v-else>
                            <img src="/assets/icons8-ok.png" height="12px" width="12px">
                            <span> Out of stock</span>
                        </template>
                    </div>
                    <div class="status dispatch-status">
                        <template v-if="shipping.props.fast_dispatch">
                            <img src="/assets/icons8-ok.png" height="12px" width="12px">
                            <span> Fast Dispatch</span>
                        </template>
                        <template v-else>
                            <img src="/assets/icons8-ok.png" height="12px" width="12px">
                            <span> No Fast Dispatch</span>
                        </template>
                    </div>
                </div>
                <div class="product-info">
                    <div><span class="product-name">{{product.name}}</span>
                    <span class="product-tag" v-for="tag in product.tags" :key="tag.key">{{tag}}</span>
                    </div>
                </div>
                <StarRating :current="reviews.rating" :countReviews="reviews.count" :buyers="reviews.total_buyers"/>
                <div class="prices">
                    <span class="big-price">{{currentPrices}}</span>
                    <span class="options">/Option <span class="options-amount">2 Options </span>(Min.Order))</span>
                    <br>
                    <span class="old-price">{{oldPrices}}</span>
                </div>
                <div class="march-expo">
                    <img alt="Match expo" src="/assets/march-expo-logo.png" height="40px" width="76px">
                    <ul class="expo-desc">
                        <li>Free shipping (up to $40)</li>
                        <li>On time delivery guaranteed</li>
                    </ul>
                    <img alt="forward" src="/assets/icons8-forward.png" height="14px" width="8px">
                </div>
                <DiscountCountdown :endDate="discount.end_date" :discountValue="discount.amount"/>
                <div class="options-box">
                    <div class="options-default-title">Options:</div>
                    <div class="options-grid">
                        <div class="option-row" v-for="option in optionKeys" :key="option.key">
                            <div class="option-title">
                                {{options[option].label}}
                            </div>
                            <div class="qty-display">
                                <span>{{options[option].price.currency.symbol}} {{(options[option].price.value).toFixed(2)}}</span>
                                <QuantityCounter v-bind:price="options[option].price.value" v-bind:startPrice="fullPrice" 
                                @changePrice ="changePrice"/>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="trade">
                    <img alt="trade" src="/assets/Icon.svg" height="14px" width="12px">
                    <span>Trade Assurance </span>
                    protects your Alibaba.com orders
                </div>
                <div class="payments">
                    Payments:
                    <img alt="visa" src="/assets/icons8-visa.svg" height="12px" width="17px">
                    <img alt="mastercard" src="/assets/icons8-mastercard.svg" height="12px" width="17px">
                    <img alt="apple" src="/assets/icons8-apple_pay.svg" height="12px" width="17px">
                </div>
                <div class="links">
                    <span>Alibaba.com Logistics</span>
                    <span>Inspection Solutions</span>
                </div>
            </div>
            <div class="info-block-3">
                <div class="info-block-3-container">
                    <div class="ship-total-price">
                        <div class="ship-to">
                            <span>Ship to <span>{{shipping.method.country}} by {{shipping.method.title}}</span></span>
                        </div>
                        <span class="full-price">{{options[optionKeys[0]].price.currency.symbol}} {{fullPrice.toFixed(2)}}</span>
                    </div>
                    <br>
                    <div class="lead-time">
                        <span>Lead time {{shipping.lead_time.value}}</span>
                        <a href="#">
                            <img alt="info" src="/assets/icons8-info.png" height="14px" width="14px"> 
                        </a>
                    </div>
                    <div class="shipping-time">
                        <span>Shipping time {{shipping.method.shipping_time.value}}</span>
                        <a href="#">
                            <img alt="info" src="/assets/icons8-info.png" height="14px" width="14px"> 
                        </a>
                    </div>
                    <div class="button-login">
                        <a href="#">
                            Login To Purchase
                        </a>
                    </div>
                    <div class="button-contact">
                        <a href="#">
                            <img alt="envelope" src="/assets/icons8-envelope.png" height="12px" width="14px"> 
                            Contact the Supplier
                        </a>
                    </div>
                </div>    
            </div>
        </section>
    </div>
</template>

<script>
import axios from 'axios';
import StarRating from './other/StarRating.vue';
import DiscountCountdown from './other/DiscountCountdown.vue';
import QuantityCounter from './other/QuantityCounter.vue';

export default {
    name:"ProductCard",
    components: {
        StarRating,
        DiscountCountdown,
        QuantityCounter
    },
    data() {
        return {
            loading: true,
            fullPrice: 0,
            product: {
                images: [], //carousel? there is an array
                name: '', 
                tags: []

            },
            options: [],
            discount: {
                amount: null,
                end_date: null
            },
            shipping: {
                props: {
                    fast_dispatch: null,
                    in_stock: null,
                    ready_to_ship: null
                },
                lead_time: {
                    info: null,
                    value: null
                },
                method: {
                    title:null,
                    country: null,
                    cost: {
                        value: null,
                        symbol: null
                    },
                    shipping_time: {
                        info: null,
                        value: null
                    }
                }
            },
            reviews: {
                rating: null,
                count: null,
                total_buyers: null
            }

        }
    },
    computed: {
        optionKeys: function() {
            const options = this.options;
            let keys = Object.keys(this.options);
            return keys;
        },
        currentPrices: function () {
            const options = this.options;
            let keys = Object.keys(this.options);
            let curr = keys[0];
            let prices = [];

            for (let i = 0; i < keys.length; i++)
            {
                let label = keys[i];
                prices.push(options[label].price.value);
            }
            prices.sort(function(a, b){return a - b});
            return `${options[curr].price.currency.symbol} ${(prices[0]).toFixed(2)} - ${options[curr].price.currency.symbol} ${(prices[prices.length-1]).toFixed(2)}`;
        },
        oldPrices: function () {
            const options = this.options;
            let keys = Object.keys(this.options);
            let curr = keys[0];
            let prices = [];

            for (let i = 0; i < keys.length; i++)
            {
                let label = keys[i];
                prices.push(options[label].old_price.value);
            }
            prices.sort(function(a, b){return a - b});
            return `${options[curr].old_price.currency.symbol} ${(prices[0]).toFixed(2)} - ${options[curr].old_price.currency.symbol} ${(prices[prices.length-1]).toFixed(2)}`;
        }
    },
    methods: {
        changePrice(value, price)
        {
            this.fullPrice = this.fullPrice + (price*value);
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

            this.options = response.data.product.options;

            this.discount.amount = response.data.product.discount.amount;
            this.discount.end_date = response.data.product.discount.end_date;

            this.shipping.props.fast_dispatch = response.data.product.shipping.props.fast_dispatch;
            this.shipping.props.in_stock = response.data.product.shipping.props.in_stock;
            this.shipping.props.ready_to_ship = response.data.product.shipping.props.ready_to_ship;
            this.shipping.lead_time.info = response.data.product.shipping.lead_time.info;
            this.shipping.lead_time.value = response.data.product.shipping.lead_time.value;
            this.shipping.method.title = response.data.product.shipping.method.title;
            this.shipping.method.country = response.data.product.shipping.method.country;
            this.shipping.method.cost.value = response.data.product.shipping.method.cost.value;
            this.shipping.method.cost.symbol = response.data.product.shipping.method.cost.currency.symbol;
            this.shipping.method.shipping_time.info = response.data.product.shipping.method.shipping_time.info;
            this.shipping.method.shipping_time.value = response.data.product.shipping.method.shipping_time.value;

            this.fullPrice = response.data.product.shipping.method.cost.value;

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
  $grey: #999999;
  $orange: #FF6600;
  $black: #333333;

    #main{
        display: flex;
        align-items:flex-start;
        margin-top: 48px;
        margin-left: 72px;
    }

    .info-block{
        display: inline-block;
    }

    .info-block-1{
        display: inline-block;
    }

    .info-block-2{
        display: inline-block;
        align-items: baseline;
        margin-left: 24px;
        margin-right: 24px;
        width: 526px;
    }

    .info-block-3{
        width: 306px;
        border-radius: 8px;
        font-size: 14px;
        line-height: 16px;
        text-align: left;
        box-shadow: 0 2px 8px 0 rgba(0, 0, 0, 0.05);
    }

    .info-block-3-container{
        align-items: center;
        padding: 24px;
    }

    .statuses{
        display:flex;    
        align-items:flex-start;
        margin-bottom: 8px;
    }

    .status{
        display: inline-block;
        border-radius: 0 2px 2px 0;
        height: 20px;
        font-size: 12px;
        line-height: 16px;
        text-align: left;
    }

    .ship-status{
        color: #FFFFFF;
        background-image: linear-gradient(270deg, #F5515F 0%, #FF7527 100%);
        border-radius: 2px 0 0 2px;
        padding-top: 2px;
        padding-right: 8px;
        padding-bottom: 2px;
        padding-left: 8px;
        display:flex;    
        align-items:center;
    }

    .stock-status{
        color: $orange;
        background-color: #FFF0E6;;
        padding-top: 2px;
        padding-right: 8px;
        padding-bottom: 2px;
        padding-left: 8px;
        display:flex;    
        align-items:center;
    }

    .stock-status img{
        margin-right: 4px;
    }

    .dispatch-status{
        color: $orange;
        background-color: #FFF0E6;
        border-radius: 0 2px 2px 0;
        padding-top: 2px;
        padding-right: 8px;
        padding-bottom: 2px;
        padding-left: 8px;
        display:flex;    
        align-items:center;
    }

    .dispatch-status img{
        margin-right: 4px;
    }

    .product-info{
        display:flex;    
        align-items:flex-start;
        text-align: left;
    }

    .product-name{
        color: $black;
        font-size: 16px;
        margin-right: 4px;
        line-height: 24px;
        width: 526px;
    }

    .product-tag{
        color: $grey;
        background-color: #EDF0F3;
        border-radius: 2px;
        font-family: Roboto;
        font-size: 12px;
        line-height: 16px;
        padding-top: 4px;
        padding-right: 8px;
        padding-bottom: 4px;
        padding-left: 8px;
        margin-bottom: 4px;
    }

    .prices{
        box-shadow: inset 0 1px 0 0 #E6E7EB, inset 0 -1px 0 0 #E6E7EB;
        padding-top: 12px;
        padding-bottom: 12px;
    }

    .big-price{
        color: $orange;
        font-weight: bold;
        font-size: 16px;
        line-height: 24px;
        text-align: left;
    }

    .options{
        color: $grey;
        margin-left: 12px;
        font-size: 14px;
        line-height: 16px;
        text-align: left;
    }

    .options-amount{
        color: $black;    
        margin-left: 12px;
    }

    .old-price{
        color: $grey;
        font-size: 14px;
        line-height: 16px;
        text-align: left;
        text-decoration: line-through;
    }

    .march-expo{
        display: inline-flex;
        align-items: center;
        width: 100%;
        margin-top: 12px;
        margin-bottom: 12px;
        background-color: #FFF0E6;
        height: 40px;
    }

    ul.expo-desc li{
        display: inline;
        color: $orange;
        font-size: 14px;
        line-height: 16px;
        text-align: left;
        list-style-type: circle;
        margin: 5px;
        padding: 2px;
    }

    .options-box{
        display: flex;
        flex-wrap: wrap;
        align-items: stretch;
        margin-top: 12px;
        font-size: 14px;
        line-height: 16px;
        justify-content: space-around;
        width:100%;
    }

    .options-default-title{
        color: $grey;
        padding-top: 12px;
        padding-bottom: 12px;
        width:20%;
    }

    .options-grid{
        width:80%;
        display: flex;
        flex-direction: column;
        align-content: flex-start;

    }

    .option-row{
        display: flex;
        flex-wrap: wrap;
        align-items: flex-start;
        justify-content: space-between;
        margin-bottom: 12px;
        text-align: center;
    }

    .option-title{
        white-space:nowrap;
        padding-top: 12px;
        padding-bottom: 12px;
    }

    .qty-display{
        text-align: left;
        display: flex;
        flex-wrap: wrap;
    }

    .qty-display span{
        padding-top: 12px;
        padding-bottom: 12px;
        padding-right:24px;
    }

    .trade{
        display: flex;
        align-items:flex-start;
        margin-top: 12px;
        margin-bottom: 12px;
        color: $grey;
        font-size: 12px;
        line-height: 16px;
    }

    .trade span{
        font-weight: bold;
        font-size: 14px;
        line-height: 16px;
        padding-left: 5px;
        padding-right: 5px;
    }

    .payments{
        display: flex;
        align-items:flex-start;
        margin-bottom: 12px;
        color: $grey;
        font-size: 12px;
        line-height: 16px;
    }

    .payments img{
        padding-left: 4px;
    }

    .links{
        color: $grey;
        font-size: 12px;
        line-height: 16px;
    }

    .links span{
        padding-right: 24px;
    }


    .button-login{
        display: inline-flex;
        justify-content: center;
        background-color: $orange;
        border-radius: 20px;
        margin-top:24px;
        width: 258px;
        height: 40px;
    }

    .ship-total-price{
        display: flex;
        flex-wrap: wrap;
        align-items: flex-start;
        justify-content: space-between;
    }

    .full-price{
        font-weight: bold;
        font-size: 16px;
        line-height: 24px;
    }

    .ship-to{
        color: $grey;
        padding: 0;
        margin: 0;
        width:132px;
        white-space: nowrap; 
        overflow: hidden;
        text-overflow: ellipsis;
    }

    .lead-time{
        color: $grey;
        display: inline-flex;
        align-items: center;
    }

    .lead-time img{
        margin-left: 12px;
    }

    .shipping-time{
        color: $grey;
        display: inline-flex;
        align-items: center;
        margin-top: 12px;
    }

    .shipping-time img{
        margin-left: 12px;
    }

    .button-login a{
        text-decoration: none;
        color: #FFFFFF;
        font-size: 14px;
        line-height: 20px;
        width: 132px;
        text-align: center;
        padding:10px;
    }

    .button-contact{
        display: inline-flex;
        justify-content: start;
        border: 1px solid $orange;
        border-radius: 20px;
        margin-top:12px;
        border-radius: 20px;
        width: 258px;
        height: 40px;
    }

    .button-contact a{
        text-decoration: none;
        color: $orange;
        font-size: 14px;
        line-height: 20px;
        text-align: center;
        padding:10px;
    }

    .button-contact img{
        margin-left: 36px;
        margin-right:12px;
    }
    

</style>