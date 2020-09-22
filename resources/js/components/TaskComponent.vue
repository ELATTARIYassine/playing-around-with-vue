<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-8">
        <div class="card">
          <div class="card-header">
            <add-task @task-added="refresh"></add-task>
          </div>
          <div class="card-body">
            <ul class="list-group">
              <li class="list-group-item d-flex justify-content-between align-items-center" 
              v-for="task in tasks.data" :key="task.id">
              <template v-if="idToEdit == task.id">
                <input class="form-control" type="text" :value="task.name" ref="taskUpdatedName">
              </template>
              <template v-else>
                {{ task.name }}
              </template>
              <template v-if="idToEdit == task.id">
                <hr width="1" size="500">
                <button class="btn btn-success" @click="updateTask(task)">update</button>
              </template>
              <template v-else>
                <button class="btn btn-info" @click="elToEdit(task)">edit</button>
              </template>
              </li>
            </ul>
            <pagination :data="tasks" @pagination-change-page="getResults" class="mt-3"></pagination>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: {},
      idToEdit: ''
    }
  },
  created() {
    axios
      .get("http://127.0.0.1:8000/task")
      .then((response) => {
        this.tasks = response.data;
      })
      .catch((err) => console.log(err));
  },
  methods: {
    getResults(page = 1) {
      axios.get("http://127.0.0.1:8000/task?page=" + page).then((response) => {
        this.tasks = response.data;
      });
    },
    refresh(tasks){
      this.tasks = tasks.data;
    },
    elToEdit(task){
      console.log(task);
      this.idToEdit = task.id;
    },
    updateTask(task){
      let newVal = this.$refs.taskUpdatedName[0].value;
      if(task.name != newVal) {
        axios.put('http://127.0.0.1:8000/task/' + task.id, {name: newVal})
          .then((res) => {
            this.idToEdit = '';
            newVal = '';
            this.tasks = res.data;
          });
      }
      else {
        this.idToEdit = '';
      }
    }
  }
};
</script>
