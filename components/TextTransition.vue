<template>
    <transition appear @before-enter="onBeforeEnter" @enter="onEnter">
        <span class="text-transition"><span>{{ text }}</span></span>
    </transition>
</template>

<style scoped>
    span{
        display: block;
    }
    .text-transition{
        overflow: hidden;
    }
</style>

<script>
import gsap from "gsap";

export default {
    props: ['text', 'delay'],
    mounted(){
        
    },
    methods: {
        onBeforeEnter(el){
            gsap.set(el.firstElementChild, { y: -parseInt(getComputedStyle(el).height) })
        },
        onEnter(el){
            console.log("before")
            el.firstElementChild.style.backgroundColor = "pink";
            gsap.to(el.firstElementChild, { y: 0, delay: this.delay, ease: "Power2.easeOut", duration: 5 })
        }
    }
}
</script>