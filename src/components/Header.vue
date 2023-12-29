<template>
    <header>
        <h1>{{ title }}</h1>
        <Button text="Add Task" color="green"></Button>
    </header>
</template>

<script>
    import Button from './Button.vue'

    export default {
        name: 'Header',
        props: {
            title: {
                type: String,
            }
        },    
        components: {
            Button
        }
    }
</script>
<!-- The 'scoped' keyword indicates a style that applies to the componet only -->
<style scoped> 
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>