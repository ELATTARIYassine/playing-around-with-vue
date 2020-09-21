<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card">
                    <div class="card-header">Tasks Component</div>

                    <div class="card-body">
                        <ul class="list-group">
                            <li class="list-group-item" v-for="task in tasks" :key="task.id">
                                {{ task.name }}
                            </li>
                        </ul>
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
        mounted() {
            console.log('Component mounted.')
        }
    }
</script>
