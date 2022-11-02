<template>
    <div id="loader" class="loader">
        <div class="load"></div>
        <span id="load-content" class="load-content">{{ textData }}</span>
        <span class="credit">Created by xSquez with <span class="red">love</span></span>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    props: ["modelValue"],
    data() {
        let textElement = document.getElementById('load-content');
        let loaderElement = document.getElementById('loader');

        return {
            textData: "Loading Page",
            text: textElement,
            loader: loaderElement
        }
    },
    mounted() {
        this.text = document.getElementById('load-content');
        this.loader = document.getElementById('loader');

        document.onreadystatechange = () => {
            if (document.readyState == "complete") { 
                setTimeout(() => {
                    this.setLoadedState();
                }, 1000)
            } 
        }
    },
    methods: {
        textTransform(text: string) {
            this.text?.classList.add('up');
            setTimeout(() => {
                this.text?.classList.add('down');
                this.text?.classList.remove('up')
                setTimeout(() => {
                    this.textData = text;
                    this.text?.classList.remove('down');
                }, 200)
            }, 200);
        },
        setLoadedState() {
            this.textTransform("Page Loaded");
            setTimeout(() => {
                this.loader?.classList.add('loaded');
                setTimeout(() => {
                    this.loader?.classList.add('no-display');
                    this.handleModel(true);
                }, 100)
            }, 1500);
        },
        handleModel(data: any) {
            this.$emit('update:modelValue', data);
        }
    },
    emits: [ 'update:modelValue' ]
})
</script>

<style lang="scss" scoped>
@use '@/style/keyframes.scss';

.loader {
    position: absolute;
    z-index: 1000;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: #121212;
    display: grid;
    place-content: center;
    transition: .1s ease;

    &.loaded {
        transform: scale(.9);
        opacity: 0;
    }

    &.no-display {
        display: none;
    }

    .credit {
        position: absolute;
        bottom: 8%;
        left: 50%;
        transform: translateX(-50%);
        font-size: .3em;
        color: #adadad;
        text-transform: uppercase;
        transition: .1s ease;

        .red {
            font-size: 1em;
            color: #a74a4a;
        }
    }

    .load-content {
        position: absolute;
        bottom: 15%;
        left: 50%;
        transform: translateX(-50%);
        font-size: .3em;
        color: #adadad;
        text-transform: uppercase;
        transition: .1s ease;

        &.up {
            bottom: 16%;
            opacity: 0;
        }

        &.down {
            bottom: 13%;
            opacity: 0;
        }
    }

    .load {
        position: relative;
        width: 1.5em;
        height: 1.5em;
        background: transparent;
        background: linear-gradient(0deg, transparent 50%, #5C5C5C 100%);
        border-radius: 100%;

        &::before {
            content: '';
            position: absolute;
            width: 80%;
            height: 80%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: #121212;
            border-radius: 100%;
            filter: blur(5px);
            z-index: 15;
        }

        &::after {
            content: '';
            position: absolute;
            width: 100%;
            height: 100%;
            background: transparent;
            background: linear-gradient(0deg, transparent 50%, #5C5C5C 100%);
            border-radius: 100%;
            z-index: 10;
            transform: scale(1.2);
            filter: blur(5px);
        }

        animation-name: spin;
        animation-duration: 1s;
        animation-timing-function: linear;
        animation-iteration-count: infinite;
    }
}
</style>