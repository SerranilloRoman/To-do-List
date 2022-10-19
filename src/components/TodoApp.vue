<script>
export default{
  data(){
    return{
      task: '',
      editedTask: null,
      availableStatuses: ['To-do', 'In-progress', 'Finished'],
      tasks:[
        {
          name: 'something2',
          status: 'to-do'
        },
        {
          name: 'kurikitaka',
          status: 'in-progress'
        }
      ]
    }
  },
  methods: {
    submitTask(){
      if(this.task.length === 0) return;

      if(this.editedTask === null){
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        });
        }else{
          this.tasks[this.editedTask].name = this.task;
          this.editedTask = null;
        }

      this.task = '';
    },
    deleteTask(index){
      this.tasks.splice(index, 1);
    },  
    editTask(index){
      this.task = this.tasks[index].name;
      this.editedTask = index;
    },
    changeStatus(index){
      let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
      if(++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.availableStatuses[newIndex];
    },
  }
};

  
</script>

<template>
  <div class='container'>
    <h2 class="text-center mt-5">To Do App</h2>
    <div>
      <input v-model="task" type="text" placeholder="Enter Task" class="form-control">
      <button @click="submitTask" class="btn btn-warning rounded-0">Submit</button>
    </div>
    <!-- table for tasks -->
    <table class="table table-bordered">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>
            <span :class="{'finished': task.status === 'Finished'}">
              {{task.name}}
          </span>
            </td>
          <td style="width: 120px" >
            <span @click="changeStatus(index)" class="pointer" :class="{
                'text-danger': task.status === 'To-do',
                'text-success': task.status === 'Finished',
                'text-warning': task.status === 'In-progress',
              }">
              {{task.status}}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editTask(index)" >
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deleteTask(index)" >
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
  .pointer{
    cursor: pointer;
  }
  .finished{
    text-decoration: line-through;
  }
</style>

