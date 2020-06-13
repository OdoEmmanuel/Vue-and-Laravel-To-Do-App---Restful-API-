    <template>
        <div class="app-component">
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
        <task-component v-for="task in tasks" :key="task.id" :task="task"></task-component>

       <tr>
       <td><input type="text" id="task" class="form-control"></td>
       <td>
        <select id="select" class="form-control">
            <option>Low</option>
            <option>Medium</option>
            <option>High</option>
        </select>
       </td>
       <td>
        <button class="btn btn-primary">ADD</button>
       </td>
       </tr>
    </tbody>
    </table>
        
    </div>
</template>

<script>
import TaskComponent from './Task.vue' ;
export default{

    data(){
        return{
            tasks: [],
            message: "Testing My Vue App" 
        }
    },

    methods: {
        getTaskData(){
            window.axios.get('/api/tasks').then(({data})=>{
                data.forEach(task => {
                   this.tasks.push(task)
                });
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