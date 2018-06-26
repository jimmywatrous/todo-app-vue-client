<template>
    <div class="container">
        <form @submit.prevent class="form-group">
            <input type="text" class="form-control" @keypress="typing=true" v-model="todo" @keyup.enter="addTodo($event)" placeholder="Add a task and press enter">
        </form>
        <ul class="collection">
            <li class="collection-item todo-item" v-for="todo in todos">
                <div class="row remove-margin">
                    <span class="left-align col s6">{{todo.task}} </span>
                    <span class="right-align col s6" ><i class="red-text material-icons" v-on:click= "deleteTodo(todo._id)">close</i></span>
                </div>
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    data () {
        return {
            todos: [],
            todo: '',
            typing: false
        }
    },
    created: function() {
        this.fetchTodos();
    },
    methods: {
        fetchTodos() {
            let uri = "http://localhost:3000";
            axios.get(uri).then((response) => {
                this.todos = response.data;
            })
        },
        deleteTodo(id) {
            console.log(id);
            let uri = "http://localhost:3000/destroy/" + id;

            axios.get(uri);
            this.fetchTodos()
        },
        addTodo(event) {
            if (event) event.preventDefault();
    
            let url = 'http://localhost:3000/add';
            let param = {

                task: this.todo


            };
  
            axios.post(url, param).then((response) => {
                this.typing=false;
                this.todo = '';
                this.fetchTodos();
            })
        }
    


    }
}
</script>

