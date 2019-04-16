<template>
    <div>
        <div class="box" id="heading">
            <h1>My Todos</h1>
        </div>
        <div class="box">
            <div class="item" v-for="todo in todos.data" :key="todo.id">
                <input type="checkbox">
                <p>{{ todo.title }}</p>
                <a href="#" @click.prevent="deleteTodo(todo.id)">x</a>
            </div>

            <form @submit.prevent>
                <input type="text" placeholder="New todo" v-model="todo.title" name="title" required>
                <button type="submit" name="list" @click="createTodo">+</button>
            </form>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        data() {
            return {
                todos: [],
                todo: {
                    id: '',
                    title: '',      
                },
                endpoint: 'api/todos'
            }
        },
        created() {
            this.fetchArticles();
        },
        methods: {
            fetchArticles() {
                axios.get(this.endpoint)
                     .then(res => this.todos = res.data)
                     .catch(err => console.log(err));
            },
            deleteTodo(id) {
                axios.delete(`${this.endpoint}/${id}`)
                     .then(res => this.fetchArticles())
                     .catch(err => console.log(err));
            },
            createTodo(){
                axios.post(`${this.endpoint}`, this.todo)
                .then(res => {
                    this.todo.title = '';
                    this.fetchArticles();
                }).catch(err => console.log(err.response.data));
            }
        }
    }
</script>