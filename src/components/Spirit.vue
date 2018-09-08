<template>
    <div :style="{right: `${right}px`, top: `${top}px` }">
        <img :src="`https://q1.qlogo.cn/g?b=qq&nk=${qq}&s=0`"/>
        <p :id="`s-${id}`" v-html="realHanashi"></p>
    </div>
</template>

<script>
    export default {
        name: "Spirit",
        props: {
            id: Number,
            top: Number,
            hanashi: String,
            qq: Number,
        },
        data() {
            return {
                right: 0,
                timer: null,
            }
        },
        computed: {
            realHanashi() {
                let tmp;
                tmp = this.hanashi.replace(
                    /</g, "&lt;"
                );
                tmp = tmp.replace(
                    />/g, "&gt;"
                );
                tmp = tmp.replace(
                    /\[CQ:image,file=.*?,url=(.*?)]/g,
                    "<img style='max-width: 10em; max-height: 10em' src='$1' referrerpolicy='no-referrer' />"
                );
                tmp = tmp.replace(
                    /\[CQ:.*?]/g,
                    ""
                );

                return tmp;
            }
        },
        mounted() {
            this.timer = setInterval(() => {
                this.right++;
                if (this.right > document.getElementById(`s-${this.id}`).clientWidth + 1024) {
                    this.$emit('nugi', this.id);
                }
            }, 5);
        },
        beforeDestroy() {
            console.log('对呀对呀');
            clearInterval(this.timer);
        }
    }
</script>

<style scoped lang="scss">
    div {
        position: absolute;
    }

    img {
        width: 4em;
        height: 4em;
        position: absolute;
        border-radius: 50%;
    }

    p {
        display: inline;
        white-space: nowrap;
        position: absolute;
        left: 5em;
    }
</style>