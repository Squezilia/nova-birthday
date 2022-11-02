<template>
    <span class="jtext loaded" v-if="loaded">
        <span class="word" v-for="(lWord, index) in words" :style="`--delay: ${index * delay}s`">{{ lWord }}&nbsp</span>
    </span>
    <span class="jtext" v-else>
        <span class="word" v-for="(uWord, index) in words" :style="`--delay: ${index * delay}s`">{{ uWord }}&nbsp</span>
    </span>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

export default defineComponent({
    props: {
        text: {
            required: true,
            type: String as PropType<string>
        },
        loaded: {
            required: false,
            default: 'loaded',
            type: Boolean as PropType<boolean>
        },
        delay: {
            required: false,
            default: .2,
            type: Number as PropType<number>
        }
    },
    data() {
        return {
            words: this.text.split(' ')
        }
    }
})
</script>

<style lang="scss" scoped>
@use '@/style/keyframes.scss';

.jtext {
    display: none;
    position: relative;
    max-width: 80%;

    .word {
        position: relative;
        opacity: 0;
        transition: all .2s ease;
        transition-delay: var(--delay);
    }

    &.toggle {
        .word {
            opacity: 1;
        }

        &.down {
            .word {
                bottom: 0;
            }
        }
        &.up {
            .word {
                top: 0;
            }
        }
        &.left {
            .word {
                left: 0;
            }
        }
        &.right {
            .word {
                right: 0;
            }
        }
    }

    &.noToggle {
        .word {
            opacity: 0 !important;
        }
    }

    &.down {
        .word {
            bottom: -.2em;
        }
    }
    &.up {
        .word {
            top: -.2em;
        }
    }
    &.left {
        .word {
            left: -.2em;
        }
    }
    &.right {
        .word {
            right: -.2em
        }
    }


    &.loaded {
        display: inline-flex;
        flex-wrap: wrap
    }
}
</style>