<template>
    <div class="app">
        <header>
            <h1>Drag & Drop</h1>
        </header>
        <router-view></router-view>
        <div class="lists">
            <div class="list" @dragover.prevent @drop="drop('todo')" :class="{ 'highlight': isHighlighted('todo') }">
                <div class="heading">
                    <div class="todoHeading">Not Started</div>
                    <div class="taskCount">
                        {{ tasks.filter(task => task.status === 'todo').length }}
                    </div>
                </div>

                <div class="list-item" v-for="item in filteredTasks('todo')" :key="item.id" draggable="true"
                    @dragstart="dragStart(item)">
                    <!-- Update here to navigate to task details -->
                    <router-link :to="'/tasks/' + item.id" class="linkItem">{{ item.content }}</router-link>
                </div>
                <input class="list-item" v-if="showNewTodoInput" type="text" v-model="newTodoItem"
                    placeholder="Add new item">
                <div class="addButton" @click="addItem('todo')">+ New</div>
            </div>
            <div class="list" @dragover.prevent @drop="drop('pending')"
                :class="{ 'highlight': isHighlighted('pending') }">
                <div class="heading">
                    <div class="pendingHeading">Pending</div>
                    <div class="taskCount">
                        {{ tasks.filter(task => task.status === 'pending').length }}
                    </div>
                </div>
                <div class="list-item" v-for="item in filteredTasks('pending')" :key="item.id" draggable="true"
                    @dragstart="dragStart(item)">
                    <!-- Update here to navigate to task details -->
                    <router-link :to="'/tasks/' + item.id" class="linkItem">{{ item.content }}</router-link>
                </div>
                <input v-if="showNewPendingInput" type="text" v-model="newPendingItem" placeholder="Add new item">
                <div class="addButton" @click="addItem('pending')">+ New</div>
            </div>
            <div class="list" @dragover.prevent @drop="drop('done')" :class="{ 'highlight': isHighlighted('done') }">
                <div class="heading">
                    <div class="doneHeading">Completed</div>
                    <div class="taskCount">
                        {{ tasks.filter(task => task.status === 'done').length }}
                    </div>
                </div>
                <div class="list-item" v-for="item in filteredTasks('done')" :key="item.id" draggable="true"
                    @dragstart="dragStart(item)">
                    <!-- Update here to navigate to task details -->
                    <router-link :to="'/tasks/' + item.id" class="linkItem">{{ item.content }}</router-link>
                </div>
                <input class="list-item" v-if="showNewDoneInput" type="text" v-model="newDoneItem"
                    placeholder="Add new item">
                <div class="addButton" @click="addItem('done')">+ New</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            tasks: [],
            draggedItem: null,
            newTodoItem: 'Todo Item',
            newPendingItem: 'Pending Item',
            newDoneItem: 'Done Item',
            showNewTodoInput: false,
            showNewPendingInput: false,
            showNewDoneInput: false
        };
    },
    mounted() {
        // Load tasks from local storage on component mount
        this.loadTasks();
    },
    methods: {
        dragStart(item) {
            this.draggedItem = item;
        },
        drop(list) {
            if (this.draggedItem) {
                this.draggedItem.status = list;
                this.draggedItem = null;
                this.saveTasks();
            }
        },
        addItem(list) {
            let newItem = { id: this.generateUUID(), content: '', status: list };
            switch (list) {
                case 'todo':
                    newItem.content = this.newTodoItem;
                    this.newTodoItem = 'Todo Item';
                    this.showNewTodoInput = false;
                    break;
                case 'pending':
                    newItem.content = this.newPendingItem;
                    this.newPendingItem = 'Pending Item';
                    this.showNewPendingInput = false;
                    break;
                case 'done':
                    newItem.content = this.newDoneItem;
                    this.newDoneItem = 'Done Item';
                    this.showNewDoneInput = false;
                    break;
            }
            this.tasks.push(newItem);
            this.saveTasks();
            const route = { path: '/tasks/' + newItem.id, component: () => import("@/pages/tasks/[id].vue") };
            this.$router.addRoute(route);
        },
        generateUUID() {
            return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
                var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
                return v.toString(16);
            });
        },
        toggleNewTodoInput() {
            this.showNewTodoInput = !this.showNewTodoInput;
        },
        toggleNewPendingInput() {
            this.showNewPendingInput = !this.showNewPendingInput;
        },
        toggleNewDoneInput() {
            this.showNewDoneInput = !this.showNewDoneInput;
        },
        filteredTasks(status) {
            return this.tasks.filter(task => task.status === status);
        },
        isHighlighted(list) {
            return this.draggedItem && this.draggedItem !== null && this.draggedItem !== undefined && this.filteredTasks(list).length > 0;
        },
        saveTasks() {
            localStorage.setItem('tasks', JSON.stringify(this.tasks));
        },
        loadTasks() {
            const savedTasks = JSON.parse(localStorage.getItem('tasks'));
            if (savedTasks) {
                this.tasks = savedTasks;
            }
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

html {
    height: 100%;
}

body {
    margin: 0;
    text-align: center;
    height: 100%;
    background-color: #e3b23c;
}

body header h1 {
    font-weight: 500;
}

.heading {
    display: flex;
    font-weight: 400;
    font-size: 16px;
}

.taskCount {
    color: gray;
    font-family: inherit;
    font-weight: 400;
    font-size: 16px;
}

.todoHeading {
    background: rgb(255, 204, 209);
    padding: 2px 5px;
    border-radius: 2px;
    margin-right: 15px;
}

.pendingHeading {
    background: rgb(251, 238, 204);
    padding: 2px 5px;
    border-radius: 2px;
    margin-right: 15px;
}

.doneHeading {
    background: rgb(204, 231, 225);
    padding: 2px 5px;
    border-radius: 2px;
    margin-right: 15px;
}

.lists {
    margin: 7rem auto;
    display: grid;
    grid-template-columns: auto auto auto;
    gap: 10px 10px;
    justify-content: space-evenly;
    align-items: center;
}

.list {
    border-radius: 4px;
    width: 200px;
    min-height: 300px;
    padding: 0.5rem;
    transform: scale(1);
    border: 3px dashed transparent;
    transition: all 0.2s;

}

.list h3 {
    font-weight: 500;
    margin: 0.5rem;
    font-size: 14px;
}

.list-item {
    background-color: #fff;
    margin: 8px auto;
    cursor: pointer;
    border-radius: 3px;
    padding: 5px 0px 5px 5px;
    box-shadow: 1px 1px 2px 1px #00000047;
}

.linkItem {
    color: black;
    font-style: normal;
    text-decoration: none;
}

.highlight {
    /* background-color: rgba(0, 0, 0, 0.3); */
    transform: scale(1.03);
}

.addButton {
    background: none;
    display: inline;
    color: gray;
    font-family: inherit;
    font-weight: 400;
    font-size: 16px;
    cursor: pointer;
}
</style>
