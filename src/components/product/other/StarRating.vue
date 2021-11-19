<template>
    <div>
        <span v-for="n in fullStars" :key="n.key">
            <img src="https://img.icons8.com/ios-filled/14/000000/filled-star.png"/>
        </span>
        <template v-if="halfStar">
            <span>
                <img src="https://img.icons8.com/ios-filled/14/000000/star-half-empty.png"/>
            </span>
        </template>
        <span v-for="i in emptyStars" :key="i.key">
            <img src="https://img.icons8.com/ios/14/000000/star--v1.png"/>
        </span>
        <span>
            {{current}}
        </span>
        <span>
            {{countReviews}} Reviews
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
        countReviews: null
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

</style>