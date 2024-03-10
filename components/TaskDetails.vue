<template>
    <div class="task-details">
        <h2>Edit Task</h2>
        <!-- Add form fields for editing task details -->
        <form @submit.prevent="saveChanges">
            <label>Title:</label>
            <input v-model="editedTask.title" required>

            <label>Status:</label>
            <select v-model="editedTask.status">
                <option value="todo">Not Started</option>
                <option value="pending">Pending</option>
                <option value="done">Completed</option>
            </select>

            <label>Description:</label>
            <textarea v-model="editedTask.description"></textarea>

            <button type="submit">Save</button>
            <button @click="deleteTask">Delete</button>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            editedTask: {
                id: null,
                title: '',
                status: '',
                description: ''
            }
        };
    },
    created() {
        // Fetch task details based on route parameter taskId and populate editedTask
        const taskId = this.$route.params.taskId;
        // Fetch task details from localStorage or API
        // Assign task details to editedTask
    },
    methods: {
        saveChanges() {
            // Save changes to the task
            // Update task details in localStorage or API
            // Redirect back to task list page
            this.$router.push('/');
        },
        deleteTask() {
            // Delete the task
            // Remove task from localStorage or API
            // Redirect back to task list page
            this.$router.push('/');
        }
    }
};
</script>

<style scoped>
/* Add styles for task details page */
.task-details {
    margin: 20px;
}

.task-details label {
    display: block;
    margin-bottom: 5px;
}

.task-details input,
.task-details select,
.task-details textarea {
    width: 100%;
    margin-bottom: 10px;
}

.task-details button {
    margin-top: 10px;
}
</style>