<template>
  <div>
      <ul>
        <li :key="todoItem.item" v-for="(todoItem, index) in todoItems" class="shadow">
          <i class="checkBtn fas fa-check" :class="{checkBtnCompleted: todoItem.completed}" v-on:click="toggleComplete(todoItem, index)"></i>
          <span :class="{textCompleted: todoItem.completed}">{{ todoItem.item }}</span>
          <span class="removeBtn" v-on:click="removeTodo(todoItem, index)">
            <i class="fas fa-trash"></i>
          </span>
        </li>
      </ul>
  </div>
</template>

<script>
export default {
  data: function(){
    return {
      todoItems: []
    }
  },
  methods:{
    removeTodo: function(todoItem, index){
      localStorage.removeItem(todoItem);
      this.todoItems.splice(index, 1); // splice : 삭제 후 새로운 배열 반환
    },
    toggleComplete: function(todoItem, index){
      todoItem.completed = !todoItem.completed;
      localStorage.removeItem(todoItem.item);
      localStorage.setItem(todoItem.item, JSON.stringify(todoItem));
    }
  },
  created: function(){
    if(localStorage.length>0){
      for(var i=0;i<localStorage.length;i++){
        if(localStorage.key(i) !== 'loglevel:webpack-dev-server'){
          this.todoItems.push(JSON.parse(localStorage.getItem(localStorage.key(i))));
        }
      }
    }
  }
}
</script>

<style scoped>
ul{
  list-style-type:none;
  padding-left: 0px;
  margin-top: 0px;
  text-align: left;
}
li{
  display: flex;
  min-height: 50px;
  height: 50px;
  line-height: 50px;
  margin: 0.5rem 0;
  padding: 0 0.9rem;
  background: white;
  border-radius: 5px;
}
.removeBtn{
  margin-left: auto;
  color: #de4343;
}
.checkBtn{
  line-height: 45px;
  color: #62acde;
  margin-right: 5px;
}
.checkBtnCompleted{
  color: #b3adad;
}
.textCompleted{
  text-decoration: line-through;
  color: #b3adad;
}
</style>