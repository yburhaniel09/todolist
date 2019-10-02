<template>
    <div>
        <h1>To-do List</h1>

        <div>
            <input type="text" v-model="newTask" placeholder="New task">
            <span class="input-group-button">
                <button class="button" @click="addTask">
                    Add
                </button>
            </span>
        </div>

        <div>
            <li v-for="(task, index) in tasks" :task="task" :key="index">
                <label @dblclick="task.edit = true" v-if="task.edit == false">{{task.title}}</label>
                <input @keyup.enter="editTask(task)" v-if="task.edit == true" type="text" v-model="task.title"/>
                <button @click="removeTask(task.id)">Delete</button>
            </li>
        </div>
    </div>
</template>

<script>
let taskId = 0

export default {
  data () {
    return {
        newTask: '',
        tasks: [
			{
                title: 'Learn Vue',
                completed: false,
                id: taskId++,
                edit: false
			}
		]
    }
  },
  methods: {
    addTask() {
      if (this.newTask) {
        this.tasks.push({
          title: this.newTask,
          completed: false,
          id: taskId++,
          edit: false
        });
        this.newTask = '';
      }
    },
    removeTask (index) {
		this.tasks = this.tasks.filter(task => {
			return task.id !== index
		})
    },
    editTask (task){
        task.edit = false;
    }
  }
}
</script>

<style scoped>

</style>
