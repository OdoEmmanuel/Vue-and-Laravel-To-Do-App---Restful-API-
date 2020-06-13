    <template>
        <div class="app-component">
<!-- <loading :active.sync="isLoading" :can-cancel="true"></loading> -->
            <table class="table">
    <thead>
        <tr>
        <th scope="col">#</th>
        <th scope="col">Task Title</th>
        <th scope="col">Priority</th>
        <th scope="col">Action</th>
        </tr>
    </thead>
    <tbody>
        <task-component v-for="task in tasks" :key="task.id" :task="task" @delete="remove"></task-component>

       <tr>
       <td><input v-model="task.title" type="text" id="task" class="form-control"></td>
       <td>
        <select v-model="task.priority" id="select" class="form-control">
            <option>Low</option>
            <option>Medium</option>
            <option>High</option>
        </select>
       </td>
       <td>
        <button @click="store()" class="btn btn-primary">ADD</button>
       </td>
       </tr>
    </tbody>
    </table>
        
    </div>
</template>

<script>
import TaskComponent from './Task.vue' ;
// import Loading from 'vue-loading-overlay';
// import 'vue-loading-overlay/dist/vue-loading.min.css';
export default{

    data(){
        return{
            tasks: [],
            task: {title: "", priority: ''}
        }
    }, 

    methods: {
        getTaskData(){
            window.axios.get('/api/tasks').then(({data})=>{
                data.forEach(task => {
                   this.tasks.push(task)
                });
            });
        },
        store(){
            if(this.validateInput()){

                window.axios.post('/api/tasks', this.task).then(savedTask =>{
                    this.tasks.push(savedTask.data);
                    this.task.title = '';
                })
            }
        },
        validateInput(){
            if(this.task.title && this.task.priority) return true
        },
        remove(id){
            this.isLoading = true;
            window.axios.delete(`/api/tasks/${id}`).then(()=>{
                let index = this.tasks.findIndex(task => task.id);
                this.tasks.splice(index, 1);
                this.isLoading = false;
            });
        }
    }, 
    created(){
        this.getTaskData();
    }, 
 
    components: {TaskComponent}
}

</script>

<style>

</style>