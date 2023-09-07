<script setup>
import {ref, reactive} from 'vue'
const todos = reactive(['be rich', 'get a girlfreind', 'retire'])
const newTodo = ref('')
const createTodo = () => {
  if(newTodo.value !== ''){
    todos.push(newTodo.value)
    newTodo.value = ''
  }
  
}

const deleteTodo = (index) => {
  let deleteItem = document.querySelectorAll('.item')[index]
  deleteItem.classList.add('dlt_animate')
  setTimeout(()=>{
    todos.splice(index, 1)
    deleteItem.classList.remove('dlt_animate')
  },900)
}

const toggleCanEdit = (index) => {
  let editItem = document.querySelectorAll('.item .content')[index]
  editItem.classList.toggle('can_edit')
  let editValue = editItem.querySelector('input').value
  todos.splice(index, 1, editValue)
}
</script>

<template>
  <div class="container">
    <h1>待辦清單</h1>
    <div class="item_create">
      <input type="text" placeholder="請輸入新增代辦事項" v-model="newTodo" @keyup.enter="createTodo">
      <button class="create" @click="createTodo">新增事項</button>
    </div>
    <ul class="todos">
      <li class="no_item" v-show="todos.length == 0">無任何代辦事項</li>
      <li class="item" v-for="(item, index) in todos" :key="index">
        <button class="edit" @click="toggleCanEdit(index)">
          <font-awesome-icon icon="fa-solid fa-pen" />
        </button>
        <div class="content">
          <span>{{ item }}</span>
          <input type="text" :value="item">
        </div>
        <div class="delete">
          <button @click="deleteTodo(index)"><font-awesome-icon icon="fa-regular fa-circle-xmark"/></button>
          
        </div>
      </li>
    </ul>
  </div>
</template>

<style lang="scss">
@import "./assets/scss/all.scss";
.container{
  width: 720px;
  height: 500px;
  margin: auto;
  padding: $sp2;
  display: flex;
  flex-direction: column;
}
h1{
  text-align: center;
  font-size: $l-font;
  color: $primary;
}
.item_create{
  display: flex;
  font-size: $s-font;
  margin-top: $sp2;
  input{
    flex-grow: 1;
    border-radius: $s-br;
    border: 2px solid #aaa;
    padding: $sp2 $sp3;
  }
}

.create{
  background: $primary;
  color: #fff;
  padding: $sp1 $sp2;
  border-radius: $s-br;
  margin-left: $sp2;
}

.todos{
  margin-top: $sp3;
  overflow: scroll;
  background-color: #fff;
  border-radius: $m-br;
  box-shadow: 5px 5px 8px rgba(0,0,0,0.2);
  flex-grow: 1;
  padding: $sp2;
}
@keyframes dltAnimate{
  0%{transform:translateX(0%) scale(1);}
  30%{transform:translateX(0%) scale(1.05);}
  70%{transform:translateX(-100%) scale(1.05);opacity:1}
  100%{transform:translateX(-150%) scale(1.05);opacity:0}
}
.item{
    display: flex;
    align-items: center;
    &:first-child{

      border-top: 1px solid #aaa;
    }
    border-bottom: 1px solid #aaa;
  &>*{
    padding: $sp2;
  }  
  &.dlt_animate{
    animation:dltAnimate 1s linear;
  }
}
.no_item{
  text-align: center;
  font-size: $m-font;
  margin-top: $sp3;
}
.edit{
  transition: .3s;
  &:hover{
    transform: scale(1.1);
  }
}
.content{
  flex-grow: 1;
  &>*{
    font-size: $m-font;
  }
  input{
    display: none;
    border: none;
    border-bottom: 1px solid #000;
  }
  &.can_edit{
    span{
      display: none;
    }
    input{
      display: block;
    }
  }
}
.delete{
  svg{
    font-size: $m-font;
    transition: .3s;
    &:hover{
      color: rgb(208, 55, 55);
      transform: scale(1.1);
    }
  }
}
</style>
