<template>
    <div  v-for="task in tasks" :key="task.id">
        <Task @toggle-reminder="$emit('toggle-reminder', task.id)" @delete-task="$emit('delete-task', task.id)" :task="task" />
    </div>
</template>

<script>
    import Task from './Task'
    export default {
        name: 'Tasks',
        props: {
            tasks: Array
        },
        components: {
            Task
        },
        // emits must be defined in chile componets when they're heing pushed up to the main app componet
        emits: ['delete-task', 'toggle-reminder'] 
    }
</script>