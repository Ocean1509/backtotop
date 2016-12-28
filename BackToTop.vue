<template>
    <div class="top" :style="`bottom:${bottom};`" v-show="visiable" @click="scrollToTop">顶部</div>
</template>
<script>
    /**
     * author wuyupeng
     * 回到顶部功能
     * */
    import getRequestAnimationFrame from './getRequestAnimationFrame';
    export default {
        name:"BackToTop",
        data(){
            return {
                visiable:false,
                visiableHeight:document.documentElement.clientHeight
            }
        },
        props:{
            onClick:{
                type:Function,
                default:()=>{}
            },
            bottom: {
                type: String,
                default: '30px'
            }
        },
        mounted(){
            this.target = document.querySelector('.common-scroll-container');
            this.target.addEventListener("scroll", this.handleScroll)
        },
        methods:{
            handleScroll(){
                this.scrollTop=this.getScroll(this.target,true);
                this.visiable=this.scrollTop>this.visiableHeight;
            },
            //获取滚动条滚动距离
            getScroll(w, top) {
                let ret;
                const method = `scroll${top ? 'Top' : 'Left'}`;
                ret = this.target[method];
                return ret;
            },
            scrollToTop(){
                const startTime = Date.now();
                const frameFunc = () => {
                    const timestamp = Date.now();
                    const time = timestamp - startTime;
                    this.setScrollTop(this.easeInOutCubic(time, this.scrollTop, 0, 450));
                    if (time < 450) {
                        getRequestAnimationFrame()(frameFunc);
                    }
                };
                getRequestAnimationFrame()(frameFunc);
            },
            setScrollTop(value) {
                this.target.scrollTop = value;
            },
            easeInOutCubic(t, b, c, d){
                const cc = c - b;
                t /= d / 2;
                if (t < 1) {
                    return cc / 2 * t * t * t + b;
                } else {
                    return cc / 2 * ((t -= 2) * t * t + 2) + b;
                }
            },
        }
    }
</script>
<style>
    .top{
        width:44px;
        height: 44px;
        background:url('../../assets/img/icon-totop.png');
        background-size:100%;
        z-index: 999;
        position: fixed;
        right: 30px;
        cursor: pointer;
        font-size: 14px;
        color:rgb(117,117,117);
        text-align: center;
        line-height: 4rem;
        background-color: #fff;
        

    }
</style>