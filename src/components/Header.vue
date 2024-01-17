<template>
    <header>
        <h1>{{ title }}</h1>
        <Button 
        v-show="homePage" 
        @btn-click="$emit('toggle-add-task')" 
        :text="showAddTask ? 'Close' : 'Add Task'" 
        :color="showAddTask ? 'red' : 'green'" 
        />
    </header>
</template>

<script>
    import Button from './Button.vue'

    export default {
        name: 'Header',
        props: {
            title: String,
            showAddTask: Boolean // See where button is embedded above for conditional text and color
        },    
        components: {
            Button
        },
        computed: {
            homePage() {
                // '/' in this instance, refers to the current page
                if(this.$route.path === '/') {
                    return true
                } else {
                    return false
                }
            }
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