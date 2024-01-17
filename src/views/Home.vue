<template>
    <div v-if="showAddTask">
      <AddTask @add-task="addTask"/>
    </div>
    <Tasks 
    @toggle-reminder="toggleReminder" 
    @delete-task="deleteTask" 
    :tasks="tasks" 
    />
</template>

<script>
    import Tasks from '../components/Tasks'
    import AddTask from '../components/AddTask'

    export default {
        name: 'Home',
        props: {
            showAddTask: Boolean,
        },
        components: {
            Tasks,
            AddTask
        },
        data() {
            return {
                tasks: [],
            }
        },
        methods: {
            // Function to POST request a newly created 'task' to the json server
            async addTask(task) {
                const res = await fetch('api/tasks', {
                    method: 'POST',
                    headers: {
                    'Content-Type': 'application/json',
                    },
                    body: JSON.stringify(task),
                })

                const data = await res.json()

                // Here, we're spreading our existing array 'this.tasks' into our passed variable 'task' to return a new array
                this.tasks = [...this.tasks, data]
            },
            // Function to DELETE request a task from the json server
            async deleteTask(id) {
                if (confirm('Are you sure?')) {
                    // Fetch request to delete task after confirming with user
                    const res = await fetch(`api/tasks/${id}`, {
                    method: 'DELETE',
                    })

                    // Deleting the task from the JSON server
                    res.status === 200 
                    ? (this.tasks = this.tasks.filter((task) => task.id !== id)) : alert('Error deleting task')

                    // Here, we're filtering out and returning an array of the remaining tasks who's id does not match the id passed in
                }
            },
            // Function to PUT request the new value of the boolean to toggle reminders
            async toggleReminder(id) {
                // We need to first await the results of fetchTask()
                const taskToToggle = await this.fetchTask(id)
                const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

                const res = await fetch(`api/tasks/${id}`, {
                    method: 'PUT',
                    headers: {
                    'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(updTask)
                })

                const data = await res.json()
                // This function uses a ternary to match the id passed in to the task.id. If the ids match, map() returns the tasks with the opposite bool, else the same bool.
                this.tasks = this.tasks.map((task) => task.id === id ? {...task, reminder: data.reminder} : task)
            },
            // Function to fetch all tasks saved in db.json
            async fetchTasks() {
                //'api' takes the place of 'http://localhost:5000' since we're defined proxies in vue.config.js
                const res = await fetch('api/tasks')

                const data = await res.json()

                return data
            },
            // Function to fetch specific task by id
            async fetchTask(id) {
                const res = await fetch(`api/tasks/${id}`)

                const data = await res.json()

                return data
            },
        },
        // Instead of hard coding our tasks, the tasks are now stored in 'db.json', and are retrieved using a GET request with 'fetch()'
        async created() {
            this.tasks = await this.fetchTasks() // When a fetch request returns a promise, you need to use the 'await' keyword
        }
    }
</script>