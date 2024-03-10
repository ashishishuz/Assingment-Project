<template>
    <div class="task-details">
        <h2>Edit Task</h2>
        <!-- Add form fields for editing task details -->
        <form @submit.prevent="saveChanges">
            <div class="headingdiv">
                <div class="textInputWrapper">
                    <label>Title:</label>
                    <input class="textInput" v-model="editedTask.content">
                </div>
                <div class="statusdiv">
                    <label>Status:</label>
                    <select v-model="editedTask.status">
                        <option value="todo">Not Started</option>
                        <option value="pending">Pending</option>
                        <option value="done">Completed</option>
                    </select>
                </div>
            </div>


            <label>Description:</label>
            <textarea class="textarea" v-model="editedTask.description"></textarea>
            <div class="buttons">
                <button class="button" type="submit">Save</button>
                <button class="button" @click="deleteTask">Delete</button>
            </div>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            editedTask: {
                id: null,
                content: '',
                status: '',
                description: ''
            }
        };
    },
    mounted() {
        // Load task details from local storage based on ID
        const taskId = this.$route.params.id;
        let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
        const task = tasks.find(task => task.id === taskId);
        if (task) {
            // If the task is found, populate the editedTask object with its details
            this.editedTask = { ...task };
        }
    },
    methods: {
        saveChanges() {
            // Save edited task details to local storage

            // Get the ID of the edited task
            const taskId = this.$route.params.id;
            // Update the task with the filled values
            let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
            const index = tasks.findIndex(task => task.id === taskId);
            if (index !== -1) {
                tasks[index].content = this.editedTask.content;
                tasks[index].status = this.editedTask.status;
                tasks[index].description = this.editedTask.description;
                localStorage.setItem("tasks", JSON.stringify(tasks));
            }
            // Redirect back to tasks list after saving changes
            this.$router.push('/');
        },
        deleteTask() {
            // Delete the task from local storage
            const taskId = this.$route.params.id;
            let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
            tasks = tasks.filter(task => task.id !== taskId);
            localStorage.setItem("tasks", JSON.stringify(tasks));
            // Redirect back to tasks list after deletion
            this.$router.push('/');
        }
    }
};
</script>

<style scoped>
* {
    box-sizing: border-box;
    font-family: 'Gill Sans', 'Gill Sans MT', Calibri, 'Trebuchet MS', sans-serif;
    /* Apply font family to all text */
}

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

.headingdiv {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 50px;
}

.statusdiv {
    display: flex;
    gap: 10px;

}

.textarea {
    width: 100%;
    height: 150px;
    padding: 12px 20px;
    box-sizing: border-box;
    border: 2px solid #ccc;
    border-radius: 4px;
    background-color: #f8f8f8;
    font-size: 16px;
    resize: none;
}

.buttons {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 20px;
}

.button {
    cursor: pointer;
    outline: 0;
    color: #fff;
    background-color: #0d6efd;
    border-color: #0d6efd;
    display: inline-block;
    font-weight: 400;
    line-height: 1.5;
    text-align: center;
    border: 1px solid transparent;
    padding: 6px 12px;
    font-size: 16px;
    border-radius: .25rem;
    transition: color .15s ease-in-out, background-color .15s ease-in-out, border-color .15s ease-in-out, box-shadow .15s ease-in-out;
}

.button:hover {
    color: #fff;
    background-color: #0b5ed7;
    border-color: #0a58ca;
}

.textInputWrapper {
    position: relative;
    display: flex;
    gap: 10px;
    align-items: center;
    width: 180px;
    margin: 12px 5px;
    --accent-color: #a3e583;
}

.textInputWrapper:before {
    transition: border-bottom-color 200ms cubic-bezier(0.4, 0, 0.2, 1) 0ms;
    border-bottom: 1px solid rgba(0, 0, 0, 0.42);
}

.textInputWrapper:before,
.textInputWrapper:after {
    content: "";
    left: 0;
    right: 0;
    position: absolute;
    pointer-events: none;
    bottom: -1px;
    z-index: 4;
    width: 100%;
}

.textInputWrapper:focus-within:before {
    border-bottom: 1px solid var(--accent-color);
}

.textInputWrapper:before {
    transition: border-bottom-color 200ms cubic-bezier(0.4, 0, 0.2, 1) 0ms;
    border-bottom: 1px solid rgba(0, 0, 0, 0.42);
}

.textInputWrapper:focus-within:before {
    border-bottom: 1px solid var(--accent-color);
    transform: scaleX(1);
}

.textInputWrapper:focus-within:after {
    border-bottom: 2px solid var(--accent-color);
    transform: scaleX(1);
}

.textInputWrapper:after {
    content: "";
    transform: scaleX(0);
    transition: transform 250ms cubic-bezier(0, 0, 0.2, 1) 0ms;
    will-change: transform;
    border-bottom: 2px solid var(--accent-color);
    border-bottom-color: var(--accent-color);
}

.textInput::placeholder {
    transition: opacity 250ms cubic-bezier(0, 0, 0.2, 1) 0ms;
    opacity: 1;
    user-select: none;
    color: rgba(255, 255, 255, 0.582);
}

.textInputWrapper .textInput {
    border-radius: 5px 5px 0px 0px;
    box-shadow: 0px 2px 5px rgb(35 35 35 / 30%);
    max-height: 36px;
    background-color: #252525;
    transition-timing-function: cubic-bezier(0.25, 0.8, 0.25, 1);
    transition-duration: 200ms;
    transition-property: background-color;
    color: #e8e8e8;
    font-size: 14px;
    font-weight: 500;
    padding: 12px;
    width: 100%;
    border-left: none;
    border-bottom: none;
    border-right: none;
}

.textInputWrapper .textInput:focus,
.textInputWrapper .textInput:active {
    outline: none;
}

.textInputWrapper:focus-within .textInput,
.textInputWrapper .textInput:focus,
.textInputWrapper .textInput:active {
    background-color: #353535;
}

.textInputWrapper:focus-within .textInput::placeholder {
    opacity: 0;
}
</style>
