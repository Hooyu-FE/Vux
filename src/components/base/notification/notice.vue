<script type="text/ecmascript-6">
    /**
     * !Vux
     * Tips消息体组件
     */
    'use strict';
    export default {
        data() {
            return {
                closeTimer: null
            }
        },
        props: {
            duration: {
                type: Number
            },
            content: {
                type: String
            },
            styles: {
                type: Object
            },
            name: {
                type: String,
                required: true
            },
            onAfter: {
                type: Function
            },
            transitionName: {
                type: String
            }
        },
        methods: {
            /**
             * 清除定时期
             */
            clearCloseTimer () {
                if (this.closeTimer) {
                    clearTimeout(this.closeTimer);
                    this.closeTimer = null;
                }
            },

            /**
             * 清除定时器
             * 执行callback
             * tips出消息队列
             */
            close () {
                this.clearCloseTimer();
                this.$parent.close(this.name, this.onAfter);
            }
        },
        mounted() {
            // 先清除存在的定时器
            this.clearCloseTimer();
            // 如果自动关闭延时不为0，设置定时器
            if (!!this.duration) {
                this.closeTimer = setTimeout(() => {
                    this.close();
                }, this.duration * 1000);
            }
        },
        beforeDestroy () {
            this.clearCloseTimer();
        }
    }
</script>

<template>
    <transition :name="transitionName" :style="{color: 'red'}">
        <!-- tips content -->
        <div :style="styles" class="tips-content-warp">
            <div v-html="content"></div>
        </div>
    </transition>
</template>

<style lang="sass">
    .tips-content-warp {
        border-radius: .3rem;
        padding: .4rem .8rem;
        text-align: center;
    }
</style>