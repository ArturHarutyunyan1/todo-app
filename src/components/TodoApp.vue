<template>
  <div>
      <div class="header">
          <img src="@/assets/bg-desktop-dark.jpg" class="desktop" alt="desktop">
          <img src="@/assets/bg-mobile-dark.jpg" class="mobile" alt="mobile">
      </div>
      <div class="container">
          <div class="content">
              <h1 class="title">TODO</h1>
              <div class="input-line">
                  <button class="add" @click="addTodo"></button>
                  <input type="text" v-model="newTodo" @keyup.enter="addTodo" placeholder="Create a new todo...">
              </div>
              <div class="todos">
                  <ul>
                      <li class="row" v-for="todo in todos" :key="todo.id">
                          <label class="row-item">
                              <input type="checkbox" name="todoItem" :checked=todo.completed @change="todo.completed = !todo.completed">
                              <span class="checkmark"></span>
                              <span class="text">{{todo.name}}</span>
                          </label>
                          <span class="remove" @click.prevent="deleteTodo(todo)"></span>
                      </li>
                      <div class="footer">
                          <h4>Challange by <a href="https://frontendmentor.io" target="_blank">Frontend Mentor</a> Coded by <a href="https://github.com/ArturHarutyunyan1" target="_blank">Artur Harutyunyan</a></h4>
                      </div>
                  </ul>
              </div>
          </div>
      </div>
  </div>
</template>

<script>
export default {
    data(){
        return{
            todos: [
                {name: "Complete online JavaScript course", completed: true},
                {name: "Jog around the park 3x", completed: false},
                {name: "10 minutes meditation", completed: false},
                {name: "Read for 1 hour", completed: false},
                {name: "Pick up groceries", completed: false},
                {name: "Complete Todo App on Frontend Mentor", completed: true}
            ],
            newTodo: ''
        }
    },
    watch:{
        todos:{
            handler(){
                localStorage.todos = JSON.stringify(this.todos)
            },
            deep: true
        }
    },
    mounted(){
        if(localStorage.todos){
            this.todos = JSON.parse(localStorage.todos)
        }
    },
    methods:{
        addTodo(){
            if(this.newTodo == ''){
                alert('You must fill in the input field')
            }else{
                this.todos.push(
                    {name: this.newTodo, completed: false}
                )
            }
            this.newTodo = ''
        },
        deleteTodo(todo) {
        const todoIndex = this.todos.indexOf(todo);
        this.todos.splice(todoIndex, 1);
        },
    }
}
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Josefin+Sans&display=swap');
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

:root{
    --very-dark-blue: hsl(235, 21%, 11%);
    --very-dark-destaurated-blue: hsl(235, 24%, 19%);
    --light-grayish-blue: hsl(234, 39%, 85%);
    --light-grayish-blue-hover: hsl(236, 33%, 92%);
    --dark-grayish-blue: hsl(234, 11%, 52%);
    --very-dark-grayish-blue: hsl(233, 14%, 35%);
}

body{
    background: var(--very-dark-blue);
    font-family: 'Josefin Sans', sans-serif;
}

.header{
    position: absolute;
    width: 100%;
    height: 300px;
    top: 0;
    left: 0;
    z-index: -1;
}

.header img{
    width: 100%;
    height: 100%;
    object-fit: cover;
}

.container{
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
}

.content{
    width: 500px;
    height: 500px;
    margin-top: 70px;
}

.title{
    text-align: center;
    font-weight: 700;
    color: white;
    letter-spacing: 5px;
}

.input-line{
    width: 100%;
    height: 60px;
    margin-top: 25px;
    background: var(--very-dark-destaurated-blue);
    border-radius: 5px;
    display: flex;
    justify-content: center;
    align-items: center;
}

input{
    width: 90%;
    height: 100%;
    background: transparent;
    border: none;
    outline: none;
    font-size: 18px;
    padding-left: 15px;
    margin-left: 10px;
    color: var(--light-grayish-blue);
}

.add{
    width: 25px;
    height: 25px;
    border-radius: 50%;
    background: transparent;
    border: 1px solid var(--dark-grayish-blue);
    cursor: pointer;
    margin-left: 25px;
}

.input-line span{
    width: 25px;
    height: 25px;
    border: 1px solid var(--font-color);
    display: block;
    border-radius: 50%;
    position: absolute;
    left: 26px;
    top: 21px;
    cursor: pointer;
}

.todos{
    width: 100%;
    height: auto;
    margin-top: 25px;
    border-radius: 5px;
    background: var(--very-dark-destaurated-blue);
}

.row-item{
    width: 100%;
    height: 60px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.text{
    font-size: 20px;
    color: var(--light-grayish-blue);
}

ul {
    list-style-type: none;
    padding: 0;
    margin: 0;
}

ul li {
    padding: 0 10px 0 25px;
    min-height: 50px;
    border-bottom: 1px solid var(--dark-grayish-blue);
    position: relative;
}

ul li input[type="checkbox"] {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    width: 0;
    height: 0;
}

ul li label {
    position: relative;
    cursor: pointer;
    padding: 2px 0 0 48px;
    font-size: 1.1rem;
}

.checkmark {
    position: absolute;
    top: 20px;
    left: 0;
    height: 25px;
    width: 25px;
}

ul li.hidden {
    display: none;
}

ul li input[type="checkbox"] ~ .checkmark {
    border: 1px solid var(--dark-grayish-blue);;
    border-radius: 50px;
}

ul li input[type="checkbox"] ~ .checkmark:hover {
    padding: 1px;
}

ul li input[type="checkbox"] ~ .checkmark:hover::before {
    content: '';
    width: 23px;
    height: 23px;
    border-radius: 50%;
    display: block;
}

ul li input[type="checkbox"]:checked ~ .checkmark {
    background: center url(../assets/icon-check.svg) no-repeat, linear-gradient(135deg, hsl(192, 100%, 67%), hsl(280, 87%, 65%));
    border: none;
}

ul li input[type="checkbox"]:checked ~ .text {
    text-decoration: line-through;
    color: var(--dark-grayish-blue);
}


.remove {
    background: url(../assets/icon-cross.svg) no-repeat;
    width: 18px;
    height: 18px;
    display: none;
    cursor: pointer;
    position: absolute;
    right: 25px;
    top: 20px;
}

ul li:hover .remove {
    display: block;
}

.footer{
    width: 100%;
    height: 70px;
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    color: var(--light-grayish-blue);
}

a{
    color: var(--dark-grayish-blue);
    text-decoration: none;
}

.mobile{
    display: none;
}

@media (max-width: 768px){
    .content{
        width: 90%;
        margin: 50px auto;
    }
    .mobile{
        display: block;
    }
    .desktop{
        display: none;
    }
}
</style>