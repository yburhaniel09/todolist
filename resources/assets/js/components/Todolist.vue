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
                <input class="taskList" v-if="task.edit == true" type="text" v-model="task.title"/>
                <button class="button" @click="task.edit = true" v-if="task.edit == false">Edit</button>
                <button class="button" @click="update(task.id, task.title)" v-if="task.edit == true">Save</button>
                <button class="button" @click="del(task.id)">Delete</button>
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
        tasks: []
    }
  },
  methods: {
    read() {
      window.axios.get('/api/cruds').then(({ data }) => {
        data.forEach(crud => {
          this.tasks.push({
            id: crud.id,
            title: crud.title,
            completed: crud.isDone,
            edit: false
          });
        });
      });
    },
    addTask() {
      if (this.newTask) {
        let addtask = {
          title: this.newTask,
          completed: false,
          id: taskId++,
          edit: false
        };
        this.tasks.push(addtask);
        window.axios.post('/api/cruds/create', {
          title: this.newTask,
          completed: false,
          userId: "1"
        }).then((response) => {
          //this.$router.push({name: 'cruds'});
          console.log('Success');
        });
      }
    },
    update(id, title) {
      window.axios.post(`/api/cruds/update/${id}`, { title }).then(() => {
        this.tasks.find(task => task.id === id).edit = false;
        console.log('Success');
      });
    },
    del(id) {
      window.axios.post(`/api/cruds/delete/${id}`).then(() => {
        let index = this.tasks.findIndex(task => task.id === id);
        this.tasks.splice(index, 1);
      });
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
