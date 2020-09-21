<template>
  <div class="container">
    <div class="row justify-content-center">
      <div class="col-md-8">
        <div class="card">
          <div class="card-header">Tasks Component</div>

          <div class="card-body">
            <ul class="list-group">
              <li class="list-group-item" v-for="task in tasks.data" :key="task.id">{{ task.name }}</li>
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
        data(){
            return {
                tasks: []
            }
        },
        created(){
            axios.get('http://127.0.0.1:8000/task')
                .then(response => {
                    this.tasks = response.data
                })
                .catch(err => console.log(err));
        },
        methods:{
            getResults(page = 1) {
			axios.get('http://127.0.0.1:8000/task?page=' + page)
				.then(response => {
					this.tasks = response.data;
				});
		}
        },
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
