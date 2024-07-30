<script setup>
const props = defineProps({
    dialog: Boolean
});

const emit = defineEmits(["update:dialog"]);
const closeDialog = () => {
    emit("update:dialog", false);
};
</script>
<template>
    <transition enter-from-class="opacity-0" leave-to-class="ease-in-out opacity-0"
        enter-active-class="transition duration-300 ease-in-out"
        leave-active-class="transition duration-300 ease-in-out">
        <div class="
                fixed 
                inset-0 
                w-screen h-auto 
                bg-gray-600 bg-opacity-50 
                top-0 left-0 z-[99] 
                flex justify-center items-center
                overflow-y-scroll" 
                role="dialog" 
                v-if="dialog" 
                @click.self="closeDialog">
            <slot></slot>
        </div>
    </transition>
</template>
