<template>
    <div class="discount-box">
        <span class="discount">{{discountValue}} OFF <span>Discount ends in:</span></span>
        <div class="countdown">
            <img alt="forward" src="/assets/icons8-clock.png" height="16px" width="16px">
            <span> {{days}}d:{{hours}}h:{{minutes}}m:{{seconds}}s</span>
        </div>
    </div>
</template>
<script>
    export default {
        name: "DiscountCountdown",
        props: {
            endDate: null,
            discountValue: null
        },
        data() {
            return {
                days: '0',
                hours: '00',
                minutes: '00',
                seconds: '00',
                expired: false
            }
        },
        methods: {
            startTimer() {
                const deadline = Date.parse(this.endDate);
                let now = new Date().getTime();
                let difference = deadline - now;
                if(difference > 0){
                    let days = Math.floor(difference / (1000 * 60 * 60 * 24));
                    let hours = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                    let minutes = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
                    let seconds = Math.floor((difference % (1000 * 60)) / 1000);

                    this.days = days;
                    this.hours = (hours < 10) ? '0' + hours : hours;
                    this.minutes = (minutes < 10) ? '0' + minutes : minutes;
                    this.seconds = (seconds < 10) ? '0' + seconds : seconds;
                } else {
                    this.expired = true;
                }
            }
        },
        mounted(){
            setInterval(this.startTimer, 1000);
        }


    }
</script>

<style lang="scss" scoped>
    .discount-box{
        display:flex;    
        align-items:center;
        margin-top: 12px;
        margin-bottom: 12px;
    }

    .discount{
        color: #FF6600;
        font-size: 14px;
        line-height: 16px;
        text-align: left;
    }

    span.discount span{
        color: #999999;
    }

    .countdown{
        display:flex;    
        align-items:center;
        margin-left:24px;
        color: #999999;
        font-size: 16px;
        font-style: italic;
        line-height: 24px;
        text-align: left;
    }

    .countdown img{
        margin-right: 10px;
    }


</style>