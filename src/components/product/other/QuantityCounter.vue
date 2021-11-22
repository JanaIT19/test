<template>
    <div class="qty-counter">
        <button type="button" class="minus" v-bind:disabled="isDisabled" v-on:click="subtractQuantity">
            <img alt="minus" src="/assets/icons8-minus.png" height="2px" width="14px">
        </button>
        <div><span>{{value}}</span></div>
        <button type="button" v-on:click="addQuantity">
            <img alt="plus" src="/assets/icons8-plus_math.png" height="14px" width="14px">
        </button>
    </div>
</template>

<script>
export default {
    name: "QuantityCounter",
    props: ["price", "startPrice"],
    data() {
        return {
            value: 0,
            isZero: true
        }
    },

    computed: {
          isDisabled: function() {
              return (this.value <= 0) ? true : false;
          }
    },

    methods: {
        subtractQuantity(){
            this.$emit("changePrice", (-1)*this.value, this.price);
            this.isZero = false;
            if (this.value > 0)
            {
                this.value -= 1;
            }
        },

        addQuantity(){
            this.value += 1;
            this.$emit("changePrice", this.value, this.price);
        },
    }


}

</script>

<style lang="scss" scoped>
    .qty-counter{
        display: flex;
        align-items:flex-end;
    }

    .qty-counter div{
        box-shadow: inset 0 1px 0 0 #E6E7EB, inset 0 -1px 0 0 #E6E7EB;
        display: inline-flex; 
        align-items: center;
        justify-content: center;
        color: #999999;
        text-align: center;
        width: 80px;
        height: 40px;
    }

    .qty-counter button{
        background-color: #FFFFFF;
        border: 1px solid #FF6600;
        border-radius: 0 2px 2px 0;
        width: 40px;
        height: 40px;
    }

    .qty-counter button:disabled{
        border: 1px solid #E6E7EB;
        border-radius: 2px 0 0 2px;
    }
</style>