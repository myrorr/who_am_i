<template>
<div class="slide">
    <Transition 
        enter-active-class="transition-opacity duration-700 ease-in-out"
        leave-active-class="transition-opacity duration-700 ease-in-out"
        enter-from-class="opacity-0"
        leave-to-class="opacity-0"
        enter-to-class="opacity-100"
        leave-from-class="opacity-100"
    >
        <slot/>
    </Transition>
</div>
</template>

<script setup lang="ts">

</script>

<style scoped>

</style>