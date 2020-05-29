<template>
<div class="todo-container">
    <div class="todo-wrap">
      <Header ref="add"></Header>
      <Main :todos="todos" :updateOne='updateOne' :deleteOne='deleteOne'></Main>
      <!-- <Footer :todos='todos' :deleteAll='deleteAll' :updateAll='updateAll'></Footer> -->
      <Footer :todos='todos' :updateAll='updateAll'></Footer>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
import Header from '@/components/Header'
import Main from '@/components/Main'
import Footer from '@/components/Footer'
export default {
    components:{
        Header,
        Main,
        Footer
    },
    mounted(){
        this.$refs.add.$on('addTodo',this.addTodo)
        this.$bus.$on('deleteAll',this.deleteAll)
    },
    data(){
        return{
            todos:JSON.parse(localStorage.getItem('todos_key')) || []

        }
    },
    methods:{
        addTodo(obj){
            this.todos.unshift(obj)
        },
        updateOne(index,val) {
            this.todos[index].isOver = val
        },
        deleteOne(index){
            this.todos.splice(index,1)
        },
        deleteAll(){
            this.todos = this.todos.filter(item => !item.isOver)
        },
        updateAll(val){
            this.todos.forEach(item => item.isOver = val)
        }
    },
    watch:{
        todos:{
            deep:true,
            handler(newVal,oldVal){
                localStorage.setItem('todos_key',JSON.stringify(newVal))
            }
        }
    }
};
</script>

<style scoped>
.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}


</style>
