<template>
  <div>
      <div class="header">
          <span v-if="lightMode === true">
              <img src="@/assets/bg-desktop-dark.jpg" alt="desktop">
          </span>
          <span v-else-if="lightMode === false">
              <img src="@/assets/bg-desktop-light.jpg" alt="desktop">
          </span>
      </div>
      <div class="container">
          <div class="content">
              <div class="space-btw">
                  <div>
                      <h1 class="title">TODO</h1>
                  </div>
                  <div>
                    <input type="checkbox" id='theme-switch' class='theme-switch' v-model="lightMode"/>
                    <label for='theme-switch'>
                        <span v-if="lightMode === true">
                            <img alt="logo" class="theme-switcher" src="@/assets/icon-sun.svg">
                        </span>
                        <span v-else>
                            <img alt="logo" class="theme-switcher" src="@/assets/icon-moon.svg">
                        </span>
                    </label>
                  </div>
              </div>
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
                          <h4>Challenge by <a href="https://frontendmentor.io" target="_blank">Frontend Mentor</a> Coded by <a href="https://github.com/ArturHarutyunyan1" target="_blank">Artur Harutyunyan</a></h4>
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
            newTodo: '',
            lightMode: false
        }
    },
    watch:{
        todos:{
            handler(){
                localStorage.todos = JSON.stringify(this.todos)
            },
            deep: true
        },
        lightMode:{
            handler(){
                let htmlEL = document.documentElement
                if(this.lightMode){
                    localStorage.setItem('theme', 'dark')
                    htmlEL.setAttribute('theme', 'dark')
                }else{
                    localStorage.setItem('theme', 'light')
                    htmlEL.setAttribute('theme', 'light')
                }
            }
        }
    },
    mounted(){
        let htmlEL = document.documentElement
        let theme = localStorage.getItem('theme')

        if(theme === 'dark'){
            htmlEL.setAttribute('theme', 'dark')
            this.lightMode = true
        }else{
            htmlEL.setAttribute('theme', 'light')
            this.lightMode = false
        }

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
  --dark-bg: #181824;
  --dark-todo-bg: #25273c;
  --dark-todo-text: #b5b7d0;
  --white: #fffeff;
  --light-bg: #fafafa;
  --light-todo-bg: #ffffff;

}
body{
  background: var(--dark-bg);
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

.space-btw{
  width: 100%;
  height: 50px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.theme-switcher{
  cursor: pointer;
}

.title{
  font-weight: 700;
  letter-spacing: 5px;
  margin-top: 17px;
  color: var(--white);
}
.input-line{
  width: 100%;
  height: 60px;
  margin-top: 25px;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  background: var(--dark-todo-bg);
}
input{
  width: 90%;
  height: 100%;
  background: transparent;
  border: none;
  outline: none;
  color: var(--dark-todo-text);
  font-size: 18px;
  padding-left: 15px;
  margin-left: 10px;
}
.add{
  width: 25px;
  height: 25px;
  border-radius: 50%;
  background: transparent;
  cursor: pointer;
  border: 1px solid var(--dark-todo-text);
  margin-left: 25px;
}
.todos{
  width: 100%;
  height: auto;
  margin-top: 25px;
  border-radius: 5px;
  background: var(--dark-todo-bg);
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
  color: var(--dark-todo-text);
}
ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}
ul li {
  padding: 0 10px 0 25px;
  min-height: 50px;
  position: relative;
  border-bottom: 1px solid var(--dark-todo-text);
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
  top: 18px;
  left: 0;
  height: 25px;
  width: 25px;
}
ul li.hidden {
  display: none;
}
ul li input[type="checkbox"] ~ .checkmark {
  border-radius: 50px;
  border: 1px solid var(--dark-todo-text);
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
  color: grey;
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
  color: var(--dark-todo-text);
}
a{
  color: royalblue;
  text-decoration: none;
}

[theme="light"] body{
  background: var(--light-bg);
}

[theme="light"] .todos{
  background: var(--light-todo-bg);
}

[theme="light"] .input-line{
  background: var(--light-todo-bg);
}

[theme="light"] .text, input{
  color: black;
}

[theme="light"] ul li{
  border-bottom: 1px solid black;
}


@media (max-width: 768px){
  .content{
    width: 90%;
    margin: 50px auto;
  }
}
</style>