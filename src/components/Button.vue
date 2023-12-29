<template>
    <!-- 'v-on:' and '@' trigger events in the same way -->
    <button v-on:click="onClick()" class="btn" :style="{ background: color }">{{ text }}</button>
</template>

<script>

    export default {
        name: 'Button',
        props: {
            text: String,
            color: String
        },
        methods: {
            onClick() {
                console.log('click');
            }
        }
    }
</script>

<style>

</style>