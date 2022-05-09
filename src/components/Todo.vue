<template>
  <div class="allComponent">
      <div>
          <h1 class="pageTitle">Todo List</h1>
      </div>

        <!--------------------- Input Section -------------------------------------->

    <div class="formSection">
        <input v-model="task" type="text" placeholder="Input your task" class="inputField" required >
        <input v-model="completeBy" type="date" placeholder="Deadline"  class="dateField" required>
        <select v-model="order" class="priorityControl" required >
            <option selected disabled="disabled" value="">Priority</option>
            <option>High</option>
            <option>Medium</option>
            <option>Low</option>
        </select>
        <button @click="saveItem" class="saveButton"> SUBMIT </button>
    </div>


            <!---------------------------- Table Section ----------------------------------------->

    <div class="tableSection">

        <table class="table">
            <thead>
                <tr>
                    <th>Task</th>
                    <th>Status</th>
                    <th>Deadline</th>
                    <th>Priority</th>
                    <th>Edit</th>
                    <th>Delete</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(item, index ) in todoItems" :key="index" >
                    <td :class="{'completed': item.status === 'Completed'}">{{toTitleCase(item.todoItem)}}</td>

                    <td @click="updateStatus(index)" class="statusSelector" :class="{'statusTodo': item.status === 'Todo', 
                    'statusOnGoing': item.status === 'On-going',
                    'statusCompleted': item.status === 'Completed' }" >{{item.status}} <i v-if="item.todoItem" class="ri-pencil-fill"></i></td>

                    <td >{{item.deadline}}</td>

                    <td>{{item.priority}}</td>

                    <td v-if="item.todoItem" @click="editItem(index)" ><i  class=" ri-pencil-fill penIcon"></i></td>

                    <td v-if="item.todoItem" @click="deleteItem(index)"><i class="ri-delete-bin-6-line"></i></td>
                    
                </tr>
            </tbody>

            
        </table>

    </div>

    <div></div>
  </div>
</template>

<script>
export default {
    data(){
        return{
            task: '',
            completeBy: '',
            order: '',

            updatedTask: null,
            updatedcompleteBy: null,
            updatedOrder: null,

            statusOptions: ['Todo', 'On-going', 'Completed'],


            todoItems:[
                {
                    todoItem:'',
                    status: '',
                    deadline: '',
                    priority: '',
                    edit: '',
                    delete: '',

                },

            ]
        }
    },

    methods:{
        saveItem(){
            if (this.task.length === 0) return;

            if (this.updatedTask === null);
            if (this.updatedcompleteBy === null);
            if (this.updatedOrder == null)
            {
                this.todoItems.push({
                todoItem: this.task,
                status: 'Todo',
                deadline: this.completeBy,
                priority: this.order,
            });
            } else{
                this.todoItems[this.updatedTask].todoItem = this.task;
                this.updatedTask = null;

                this.todoItems[this.updatedcompleteBy].deadline = this.completeBy;
                this.updatedcompleteBy = null;

                this.todoItems[this.updatedOrder].priority = this.order;
                this.updatedOrder = null;
            }
            

            this.task = '';
            this.completeBy = '';
            this.order = '';

            
        },

        deleteItem(index){
            this.todoItems.splice(index, 1);
        },

        editItem(index){
            this.task = this.todoItems[index].todoItem;
            this.completeBy = this.todoItems[index].deadline;
            this.order = this.todoItems[index].priority;

            this.updatedTask = index;
            this.updatedcompleteBy = index;
            this.updatedOrder = index;
        },

        updateStatus(index){
            let newIndex = this.statusOptions.indexOf(this.todoItems[index].status);
            // ++newIndex
            if (++newIndex > 2) newIndex = 0;
            this.todoItems[index].status = this.statusOptions[newIndex];
            console.log(newIndex)
        },

        toTitleCase(str){
            return str.charAt(0).toUpperCase() + str.slice(1);
        }

    }
}
</script>

<style>

.formSection{
    display: flex;
    justify-content:space-between;
    align-items: center;
    width: 700px;
    margin: auto;
    background: #E6E6FA;
    padding: 12px;
    border-radius:20pt;
}

.inputField{
    display: flex;
    width: 35%;
    height: 42px;
    /* border-radius:40pt 0pt 0pt 40pt; */
    border-radius:10pt;
    border-color: gray;
    box-shadow: inset 1px 2px 8px rgba(0, 0, 0, 0.07);
    padding: 0px 16px;
    font-size: 12pt;
}

.dateField{
   height: 46px; 
   background:  white;
   /* border: none; */
   border-radius:10pt;
   color: black;
   padding: 0px 12px;
}

.priorityControl{
     height: 46px;
     padding: 0px 12px;
     border-radius:10pt;
}

.saveButton{
    padding: 0px 12px;
    width: 100px;
    height: 46px;
    border-radius:10pt;
    border:none;
    background: blue;
    color: white;
    font-weight: bold;
    font-size: 12pt;
    box-shadow: 0 0 30px rgba(0, 0, 0, 0.15);
    
}

.pageTitle{
    color: black;
}

.tableSection{
    margin-top: 36px;
}

.table{
    margin: auto;
    border-collapse: collapse;
    /* margin: 25px 0; */
    font-size: 0.9em;
    font-family: sans-serif;
    min-width: 400px;
    width: 700px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}

.table thead tr {
    background-color: blue;
    color: #ffffff;
    text-align: left;
}

table th,
.table td {
    padding: 12px 15px;
    text-align: left;
}

.table tbody tr {
    border-bottom: 1px solid #dddddd;
}

.table tbody tr:nth-of-type(even) {
    background-color: #f3f3f3;
}

.table tbody tr:last-of-type {
    border-bottom: 2px solid blue;
}

.statusSelector{
    cursor: pointer;
    font-weight: bold;
}

.completed{
    text-decoration: line-through;
    /* font-weight: bold; */
}

.statusTodo{
    color: red;
}

.statusOnGoing{
    color: purple;
}

.statusCompleted{
    color: green;
}

.penIcon{
    color: blue;
}

.ri-delete-bin-6-line{
    color: red;
}
</style>