<template>
    <div class="ratingselect">
        <div class="rating-type">
            <span @click="select(2)" class="block positive" :class="{'active':selectType===2}">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
            <span @click="select(0)" class="block positive" :class="{'active':selectType===0}">{{desc.positive}}<span class="count">{{positives.length}}</span></span>
            <span @click="select(1)" class="block negative" :class="{'active':selectType===1}">{{desc.negative}}<span class="count">{{negatives.length}}</span></span>
        </div>
        <div class="switch">
            <span @click="toggleContent" class="icon" :class="{'check':onlyContent}"></span>
            <span class="text">只看有内容的评价</span>
        </div>
    </div>
</template>

<script>
    const POSITIVE = 0;
    const NEGATIVE = 1;
    const ALL = 2;

    export default {
        data () {
            return {
                selectType: ALL,
                onlyContent: false
            }
        },
        props: {
            ratings: {
                type: Array,
                default () {
                    return [];
                }
            },
            desc: {
                type: Object,
                default () {
                    return {
                        all: '全部',
                        positive: '满意',
                        negative: '不满意'
                    }
                }
            }
        },
        computed: {
            positives () {
                return this.ratings.filter(rating => {
                    return rating.rateType === POSITIVE;
                })
            },
            negatives () {
                return this.ratings.filter(rating => {
                    return rating.rateType === NEGATIVE;
                })
            }
        },
        methods: {
            select (type) {
                this.selectType = type;
                this.$emit('ratingtype:select', type);
            },
            toggleContent () {
                this.onlyContent = !this.onlyContent;
                this.$emit('content:toggle', this.onlyContent);
            }
        }
    }
</script>

<style scoped>
    .ratingselect {

    }
    .ratingselect .rating-type {
        padding: 18px 0;
        margin: 0 18px;
        border-bottom: 1px solid rgba(7, 17, 27, 0.1);
    }

    .block {
        display: inline-block;
        padding: 8px 12px;
        margin-right: 8px;
        border-radius: 1px;
        font-size: 12px;
        color: rgb(77, 85, 93);
    }
    .block .count {
        font-size: 8px;
        margin-left: 2px;
        line-height: 12px;
    }
    .block.active {
        color: #fff;
    }
    .block.positive {
        background-color: rgba(0, 160, 220, 0.2);
    }
    .block.negative {
        background-color: rgba(77, 85, 93, 0.2);
    }
    .positive.active {
        background-color: rgb(0, 160, 220);
    }
    .negative.active {
        background-color: rgb(77, 85, 93);
    }
    .ratingselect .switch {
        padding: 12px 18px;
        line-height: 24px;
        border-bottom: 1px solid rgba(7, 17, 27, 0.1);
        color: rgb(147, 153, 159)
    }
    .switch .icon {
        display: inline-block;
        width: 24px;
        height: 24px;
        margin-right: 4px;
        background-image: url(./check.svg);
        background-size: 24px 24px;
    }
    .switch .icon.check {
        background-image: url(./check_full.svg);
    }
    .switch .text {
        font-size: 12px;
        line-height: 24px;
        vertical-align: top;
    }
</style>

