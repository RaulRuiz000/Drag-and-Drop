<script setup>
import { ref } from 'vue';
import InputBoard from './InputBoard.vue';
import MenuTask from './MenuTask.vue';
import InputEditTask from './InputEditTask.vue';
const boards = ref([
    {
        idBoard: crypto.randomUUID(),
        title: 'tablero 1',
        task: [
            {
                idTask: crypto.randomUUID(),
                content: 'tarea1',
                statusEdit: false
            },
            {
                idTask: crypto.randomUUID(),
                content: 'tarea2',
                statusEdit: false
            },
            {
                idTask: crypto.randomUUID(),
                content: 'tarea3',
                statusEdit: false
            }
        ]
    },
    {
        idBoard: crypto.randomUUID(),
        title: 'tablero 2',
        task: [
            {
                idTask: crypto.randomUUID(),
                content: 'tarea4',
                statusEdit: false
            },
            {
                idTask: crypto.randomUUID(),
                content: 'tarea5',
                statusEdit: false
            },
            {
                idTask: crypto.randomUUID(),
                content: 'tarea6',
                statusEdit: false
            }
        ]
    }
]);
let draggedItem = null
let boardId = null
const dragStart = (task, id) => {
    draggedItem = task
    boardId = id
}
const EditTask = ref('')
const dragOver = (event) => {
    event.preventDefault();
}
const drop = (event, id) => {
    event.preventDefault();
    if (draggedItem) {
        boards.value.forEach(item => {
            if (item.idBoard == id) {
                item.task.push(draggedItem)
                const originBoard = boards.value.find(item => item.idBoard == boardId)
                const indexTask = originBoard.task.findIndex(item => item.idTask == draggedItem.idTask)
                if (indexTask >= 0) originBoard.task.splice(indexTask, 1)
                draggedItem = null
                boardId = null
            }
        });
    }
};
const createBoard = (value) => {
    const newboard = {
        idBoard: crypto.randomUUID(),
        title: value,
        task: []
    }
    boards.value.push(newboard)
};
const createItem = (text, board) => {
    const newTask = {
        idTask: crypto.randomUUID(),
        content: text,
        statusEdit: false
    }
    board.task.push(newTask)
};
const menu = (text, board, task) => {
    if (text == 'Eliminar') {
        const taskIndex = board.task.findIndex(item => item.idTask == task.idTask)
        if (taskIndex >= 0) board.task.splice(taskIndex, 1)
    }
    if (text == 'Editar') {
        task.statusEdit = true
    }
};
const updateTask = (value, task, board) => {
    if (value.edit) {
        for (let i = 0; i < board.task.length; i++) {
            if (board.task[i].idTask == task.idTask) {
                board.task[i].content = value.content
                break
            }
        }
    }
    task.statusEdit = false
};
</script>

<template>
    <div class="header">
        <div class="tittle">
            <h1>Cree su tablero canvas</h1>
        </div>
        <div class="input">
            <InputBoard @new-board="createBoard" :label="'Nuevo tablero'" />
        </div>
    </div>

    <div class="board-container">

        <div class="board" v-for="board in boards" :key="board.idBoard" @dragover="dragOver"
            @drop="drop($event, board.idBoard)">
            <div class="header-board">
                <h3>{{ board.title }}</h3>
                <InputBoard @new-board="(text) => createItem(text, board)" :label="'Crear nueva tarea'" />
            </div>

            <div class="items">
                <div class="item" v-for="task in board.task" draggable="true" @dragstart="dragStart(task, board.idBoard)">
                    <div class="menu">
                        <MenuTask @option-menu="(text) => menu(text, board, task)" />
                    </div>
                    <p v-show="!task.statusEdit">{{ task.content }}</p>
                    <div v-show="task.statusEdit">
                        <InputEditTask :title-task="task.content"
                            @value-edit-task="(value) => updateTask(value, task, board)" />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped>
.board-container {
    width: 100vw;
    height: 60vh;
    background-color: #282E33;
    display: flex;
    padding: 1em;

    overflow-x: auto;
}

.board-container::-webkit-scrollbar {
    height: 7px;
    background-color: #1C2015;
    border-radius: 5px;
}

.board-container::-webkit-scrollbar-thumb {
    background-color: #323B36;
    border-radius: 5px;
}

.header {
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
    height: 40vh;
    background-color: cadetblue;
    color: white;
}

.header .input {
    width: 50%;
}


.board {
    background-color: #101204;
    margin-right: 1em;
    width: 300px;
    height: 100%;
    text-align: center;
    overflow-y: auto;
    overflow-x: hidden;
    flex-shrink: 0;
    position: relative;
    border-radius: 2px;
}

.header-board {
    position: sticky;
    z-index: 2;
    width: 95%;
    margin: 0 0.5em;
    padding-top: 1em;
    padding-bottom: 0.5em;
    top: 0;
    color: white;
    background-color: #101204;
}

.board::-webkit-scrollbar {
    width: 7px;
    background-color: #1C2015;
    border-radius: 5px;
}

.board::-webkit-scrollbar-thumb {
    background-color: #323B36;
    border-radius: 5px;
}

.item {
    color: white;
    background-color: #282E33;
    margin: 1em 0.5em;
    padding: 1em;
    cursor: pointer;
    position: relative;
}

.menu {
    position: absolute;
    top: 1%;
    right: 0;
}

@media (max-width: 455px){
    .tittle{
        font-size: 0.8em;
    }
    .input{
        margin-top: 1em;
    }
    .header{
        justify-content: center;
    }
    
}

</style>