<template>
  <transition name="todo" appear>
    <li>
        <label>
          <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)"/>
          <span v-show="!todo.isEdit">{{todo.title}}</span>
          <input  
          type="text" 
          ref="inputTitle"
          v-show="todo.isEdit" 
          :value="todo.title" 
          @blur="handleBlur(todo,$event)">
        </label>
        <button class="btn btn-danger" @click="deleteItem(todo.id)">删除</button>
        <button class="btn btn-edit" v-show="!todo.isEdit" @click="handleEdit(todo)">编辑</button>
    </li>
  </transition>
</template>

<script>
export default {
    name:'MyItem',
    props:['todo'],
    methods: {
      handleCheck(id){
        this.$bus.$emit('checkTodo',id)
      },

      deleteItem(id){
        if(confirm('确定删除吗？')){
          this.$bus.$emit('deleteTodo',id)
        }
      },
      handleEdit(todo){
        todo.isEdit = true
        this.$nextTick(function(){
          this.$refs.inputTitle.focus()
          
        })
      },
      handleBlur(todo,e){
        todo.isEdit = false 
        if(! e.target.value.trim()) return alert("输入不能为空！")
        this.$bus.$emit('updateTodo',todo.id,e.target.value)
      }
    },
}
</script>

<style scoped>
  /*item*/
  li {
    list-style: none;
    height: 36px;
    line-height: 36px;
    padding: 0 5px;
    border-bottom: 1px solid #ddd;
  }

  li label {
    float: left;
    cursor: pointer;
  }

  li label li input {
    vertical-align: middle;
    margin-right: 6px;
    position: relative;
    top: -1px;
  }

  li button {
    float: right;
    display: none;
    margin-top: 3px;
  }

  li:before {
    content: initial;
  }

  li:last-child {
    border-bottom: none;
  }

  li:hover{
    background-color: #ddd;
  }

  li:hover button{
    display: block;
  }

  /* 动画效果 */
  .todo-enter-active{
    animation: a 1s linear;
  }
  .todo-leave-active{
    animation: a 1s linear reverse;

  }
  @keyframes a {
    from{
      transform: translateX(-100%);
    }
    to{
      transform: translateX(0px);
    }
  }

</style>