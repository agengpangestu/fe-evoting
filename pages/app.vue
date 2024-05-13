<script setup>
import SideBarComp from '@/components/SideBarComp.vue';
</script>
<template>
    <div class="relative flex w-full h-max p-2.5">
        <div class="w-[180px] h-full mr-1.5">
            <SideBarComp />
        </div>
        <div class="w-screen min-h-screen ml-8">
            <NuxtPage />
        </div>
    </div>
</template>
