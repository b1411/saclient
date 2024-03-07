<template>
    <div class="bg-slate-700 max-w-4xl rounded-xl mx-auto py-20 px-5 md:px-0">
        <div class="mx-auto lg:max-w-xl md:max-w-lg max-w-[300px]">
            <h2 v-if="props.title" class="text-xl font-bold border-b-[1px] border-primary pb-3 mb-5">{{ props.title }}</h2>
            <div class="flex flex-col gap-y-5">
                <slot name="inputs"></slot>
            </div>
        </div>
    </div>
</template>

<script setup>
const props = defineProps({
    title: String
})

</script>