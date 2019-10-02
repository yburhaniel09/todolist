<template>
    <div>
        <div class="add">
            <input class="taskList" type="text" v-model="newTask" placeholder="New task">
            <span class="input-group-button">
                <button class="button" @click="addTask">
                    Add
                </button>
            </span>
        </div>

        <div class="list">
            <li v-for="(task, index) in tasks" :task="task" :key="index">
                <label class="taskList" v-if="task.edit == false">{{task.title}}</label>
                <input class="taskList" @keyup.enter="editTask(task)" v-if="task.edit == true" type="text" v-model="task.title"/>
                <button class="button" @click="task.edit = true" v-if="task.edit == false">Edit</button>
                <button class="button" @click="task.edit = false" v-if="task.edit == true">Save</button>
                <button class="button" @click="removeTask(task.id)">Delete</button>
            </li>
        </div>

        <div class="save">
            <button class="savebutton">Save To Do List</button>
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
    read() {
      window.axios.get('/home').then(({ data }) => {
        console.log(data)
      });
    },
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
  },
  created() {
    this.read();
  }
}
</script>

<style scoped>
  div.add {
    text-align: center;
  }

  div.list {
    margin-top: 10px;
  }

  div.save {
    margin-top: 10px;
    text-align: center;
  }

  li {
    list-style-type: none;
  }

  .taskList {
    width: 550px;
  }

  .button {
    width: 80px;
  }

  .savebutton {
    width: 120px;
  }


</style>
