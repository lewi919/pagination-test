<template>
    <div ref="target" class="w-full flex justify-center items-center">
        <div class="loader rounded-full w-[50px] h-[50px] border-solid border-8 text-blue-100 border-t-blue-600"></div>
    </div>
</template>

<script setup>
    import { onMounted, ref, onBeforeUnmount } from 'vue'

    let observer
    const target = ref()
    const emits = defineEmits(['intersect'])

    onMounted(() => {
        observer = new IntersectionObserver(([entry]) => {
            if (entry.isIntersecting) {
                emits('intersect')
            }
        })
        observer?.observe(target.value)
    })

    onBeforeUnmount(() => {
        observer?.disconnect()
    })
</script>

<style scoped>
    .loader {
        -webkit-animation: spin 1.5s linear infinite;
        animation: spin 1.5s linear infinite;
    }

    @-webkit-keyframes spin {
        0% {
            -webkit-transform: rotate(0deg);
        }

        100% {
            -webkit-transform: rotate(360deg);
        }
    }

    @keyframes spin {
        0% {
            transform: rotate(0deg);
        }

        100% {
            transform: rotate(360deg);
        }
    }
</style>