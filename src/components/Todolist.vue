<template>
  <div class="master">

    <input type="text" class="todo-input" placeholder="Add things that need to be done!" 
    v-model="newToDo" @keyup.enter="addTodo()">
    <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
    <div v-for="(todo,index) in todos" :key="todo.id" class="todo-item">
        
        <div class="todo-item-left">
            <input type="checkbox" class="checkbox-spin" v-model="todo.completed" @change="saveTodo()">
            <div v-if="!todo.editing" @dblclick="todoEdit(todo)" class="todo-item-label" :class="{ticked: todo.completed}">
                {{todo.title}}
            </div>
            <input v-else class="todo-item-edit" type="text" v-model="todo.title" 
            @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" @keyup.esc="cancelEdit(todo)" v-focus>     
        </div>

        <div class="remove-item" @click="removeToDo(index)"> 
            &times;
        </div>
    
    </div>
    </transition-group>
    <div class="bottom-container">
        Remaining tasks: {{todoCounter}}
    </div>

  </div>
</template>

<script>

const focus = {
  mounted: (el) => el.focus()
}
 
export default {

  data () {
    return {

        newToDo: '',
        idForTodo: 1,
        beforeEdit: '',
        counter: 0,
        todos: [],

    }
    },

    directives: {
        focus
    },

    methods: {
        addTodo() {

            if (this.newToDo.trim().length == 0) {
                return
            }
            
            this.todos.push({
                id: this.idForTodo,
                title: this.newToDo,
                completed: false,
                editing: false
            }) 

            this.saveTodo();
            

            this.newToDo = "";
            this.idForTodo++;
            
        },

        saveTodo() {
            const parsed = JSON.stringify(this.todos);
            localStorage.setItem('todos',parsed);
            
        },

        removeToDo(index) {
            this.todos.splice(index,1);
            this.saveTodo();
            
        },

        todoEdit(todo) {
            if(todo.completed) {
                return
            }
            this.beforeEdit = todo.title;
            todo.editing = true;
        },

        doneEdit(todo) {
            if (todo.title.trim() == '') {
                todo.title = this.beforeEdit;
            }
            this.saveTodo();
            todo.editing = false;
        },

        cancelEdit(todo) {
            todo.title = this.beforeEdit;
            todo.editing = false;
        },
        
        

    },
    computed: {
        todoCounter() {
            var count = 0;
            
            for(let i=0; i<this.todos.length; i++) {
                if(!this.todos[i].completed) {
                    console.log("triggr");
                    count++;
                }
            }
            return count;
        }
    },
    

    mounted() {

        if (localStorage.getItem('todos')) {
            try {
                this.todos = JSON.parse(localStorage.getItem('todos'));
            } catch(e) {
                localStorage.removeItem('todos');
            }
        }
        
    }
    
}
</script>


<style>

@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.5.2/animate.min.css");

.todo-input{
    width: 400px;
    padding: 10px 18px;
    font-size: 18px;
    border-radius: 5px;
}

.todo-item {
    width: 435px;
    margin-bottom:12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    font-size: 24px;
    
}

.remove-item {
    cursor: pointer;
    margin-left: 14px;
}

.remove-item:hover {color: red;}


.todo-item-left{
    font-size: 20px;
    display: flex;
    align-items: center;
}

.todo-item-label{
    padding: 10px;
    border: 1px solid white;
    margin-left: 0px;
    cursor: pointer;
   
}

.ticked{
    text-decoration: line-through;
}

.todo-item-edit {
    font-size: 22px;
    color: #2c3e50;
    margin-left: 12px;
    width: 100%;
    padding: 7px;
    border: 1px solid #ccc;
    font-family: 'Avenir', Arial, Helvetica, sans-serif;
    border-radius: 5px;
}

.checkbox-spin{
    display:inline-block;
    width:25px;
    height:17px;
    margin: 0 5px -4px 0; /*layout relationship between check and label*/
}

.bottom-container{
    width: 435px;
    display: flex;
    align-items: center;
    border-top: 2px solid lightgray;
    padding-top: 14px;
    font-size: 18px;
}

</style>

