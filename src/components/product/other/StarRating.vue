<template>
    <div class="rating-box">
        <span v-for="n in fullStars" :key="n.key">
            <img src="/assets/filled-star.png"/>
        </span>
        <template v-if="halfStar">
            <span>
                <img src="/assets/star-half-empty.png"/>
            </span>
        </template>
        <span v-for="i in emptyStars" :key="i.key">
            <img src="/assets/star--v1.png"/>
        </span>
        <span class="rating">
            {{current}}
        </span>
        <span class="reviews-count">
            {{countReviews}} Reviews
        </span>
        <span class="buyers-count">
            {{buyers}} Buyers
        </span>
    </div>
</template>

<script>
function calculateStars(current, fill) {
    switch(fill){
        case 'full':
            return Math.floor(current);
        case 'half':
            let countHalf = current - Math.floor(current);
            if(countHalf > 0)
            {
                return true;
            }
            return false;
        case 'empty':
            return (5 - Math.ceil(current));
    }
}

export default {
    name: "StarRating",
    props: {
        current: null,
        countReviews: null,
        buyers: null
    },
    data() {
        return {
           fullStars: calculateStars(this.current, 'full'),
           halfStar: calculateStars(this.current,'half'),
           emptyStars: calculateStars(this.current, 'empty')
        }
    }
}
</script>

<style lang="scss" scoped>

.rating-box{
    display: flex;
    flex-wrap: wrap;
    font-size: 12px;
    line-height: 16px;
    text-align: left;
    margin-top: 12px;
    margin-bottom: 12px;

}

.rating-box span{
    margin-right: 4px;
}

.rating{
    color: #FF6600;
}

.reviews-count{
    color: #999999;
}

.buyers-count{
    color: #333333;
    margin-left: 24px;
}



</style>