<template>
    <div>
        <h1 class="todo-header">Welcome {{title}}</h1>
        <p><button class="todo-btn" v-on:click="isShow = true">ADD A ToDo</button></p>
        <div v-if="isShow">
                <input type="text" v-model="initailToDo.todoText" placeholder="Input your To Do">
                <br>
                <input type="date" v-model="initailToDo.todoDate">
                <br>
                <button type="submit" v-on:click="submitToDo">Submit</button>
        </div>

        <table id="todo-table">
            <caption>Your All ToDos List</caption>
            <tr>
                <th>Title</th>
                <th>Time</th>
                <th>Status</th>
                <th>Action</th>
            </tr>
            <tr v-for="(todo , index) in todos" :key="index">
                <td>{{todo.text}}</td>
                <td>{{todo.date}}</td>
                <td>
                    <span class="pointer" @click="changeStatus(index)">
                        {{todo.status}}
                    </span>
                </td>
                <td>
                    <button @click="editTodo(index)" class="btn-action"><i class="fa-solid fa-pen-to-square"></i></button>
                    <button @click="deleteTodo(index)" class="btn-action"><i class="fa-solid fa-trash"></i></button>
                </td>
            </tr>
        </table>
    </div>
</template>

<script>
export default {
    name: "ToDo",
    props: ['title'],
    data(){
        return{
            initailToDo: {
                todoText: '',
                todoDate: null
            },
            editToDo: null,
            todos: [],
            availablestatus: ['Pending','On-going','Done'],
            isShow: false,

        }
    },

    methods:{
        submitToDo(){
            this.isShow = false;
            if(this.initailToDo.todoText.length === 0) return;
            if(this.initailToDo.todoDate === null) return;
            if(this.editToDo === null)
            {
                this.todos.push({
                    text: this.initailToDo.todoText,
                    status: 'Pending',
                    date: this.initailToDo.todoDate,
                })
            }else{
                this.todos[this.editToDo].text = this.initailToDo.todoText;
                this.todos[this.editToDo].date = this.initailToDo.todoDate;
                this.editToDo = null;
            }
            
            this.initailToDo.todoText= '',
            this.initailToDo.todoDate = null
        },

        deleteTodo(index){
            this.todos.splice(index,1);
        },

        editTodo(index){
            this.isShow = true;
            this.initailToDo.todoText = this.todos[index].text;
            this.initailToDo.todoDate = this.todos[index].date;
            this.editToDo = index;
        },
        
        changeStatus(index){
            let newIndex = this.availablestatus.indexOf(this.todos[index].status)
            if(++newIndex > 2){
                newIndex = 0;
            }
            this.todos[index].status = this.availablestatus[newIndex];
        }
    }

}
</script>

<style scoped>
    .todo-header{
        background-color: aquamarine;
        padding: 10px;

    }

    .todo-btn{
        border-radius: 10px;
        background-color: greenyellow;
        padding: 10px 20px;
        border: 2px solid red;
    }

    #todo-table caption{
        font-size: 30px;
        margin-top: 30px;
        padding: 10px;
        font-weight: bold;
    }

    .pointer{
        cursor: pointer;
    }

    #todo-table th {
    padding-top: 12px;
    padding-bottom: 12px;
    text-align: center;
    background-color: #04AA6D;
    color: white;
    }

    #todo-table {
    font-family: Arial, Helvetica, sans-serif;
    border-collapse: collapse;
    width: 100%;
    }

    #todo-table td, #todo-table th {
    border: 1px solid #ddd;
    padding: 8px;
    }

    #todo-table tr:nth-child(even){background-color: #f2f2f2;}

    #todo-table tr:hover {background-color: #ddd;}

    input[type=text],input[type=date], select {
        width: 50%;
        padding: 12px 20px;
        margin: 8px 0;
        display: inline-block;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
    }

    button[type=submit] {
    width: 50%;
    background-color: #4CAF50;
    color: white;
    padding: 14px 20px;
    margin: 8px 0;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    }

    button[type=submit]:hover {
    background-color: #45a049;
    }

    .btn-action{
        color: #04AA6D;
        margin-right: 20px;
        font-size: 1.2rem;
    }
</style>