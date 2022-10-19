<template>
  <h1 class="text-joshuaBlue mb-10">{{ msg }}</h1>
  <div class="todo-container">
    <input v-model.trim="task" class="task-input" type="text" placeholder="Enter The Task ...">
    <button @click="addTask">Submit</button>
    <div v-for="(item, index) in taskList" :key="index" class="list-area">
      <div class="list-info">
        <input class="finishCheck" v-model="item.isFinish" type="checkbox">
        <div @click="editTask(index)" :class="{'finished' : item.isFinish === true}">
          {{ item.taskName }}
        </div>
        <input class="ml-4" v-if="item.isEdit" v-model.trim.lazy="taskList[index].taskName" type="text">
        <div class="text-joshuaBlue border rounded-lg mx-2 cursor-pointer" v-if="item.isEdit" @click="edited(index)">Edit</div>
      </div>
      <button @click="deleteTask(index)">Delete</button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch, onMounted } from 'vue'
  // import { useStore } from '../store/index'
  defineProps<{ msg: string }>()

  const task = ref('')

  const taskList = ref<any[]>([])
  
  onMounted(() => {
    const data = localStorage.getItem('taskList');
    if (data === null) {
      []
    } else {
      taskList.value = JSON.parse(data)
    }
  })

  function addTask () {
    if (task.value.length === 0) {
      alert ('Please Enter A Task!!!')
      // console.log(task.value)
      return
    } else {
      taskList.value.push({
        taskName: task.value,
        isFinish: false,
        isEdit: false
      })
      task.value = ''
    }
  }

  function editTask (index:number) {
    console.log(taskList.value[index].taskName)
    taskList.value[index].isEdit = true
  }

  function edited (index:number) {
    taskList.value[index].isEdit = false
  }

  function deleteTask (index:number) {
    taskList.value.splice(index, 1)
  }

  watch(taskList, (newValue) => {
    console.log(newValue)
    localStorage.setItem('taskList', JSON.stringify(taskList.value))
    console.log('OK')
  }, { deep: true })

</script>

<style scoped>
.read-the-docs {
  color: #888;
}
.todo-container {
  border: 2px solid skyblue;
  border-radius: 8px;
  padding: 10px;
}

.task-input {
  margin-right: 10px;
}

.list-area {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid gray;
  padding: 10px 4px;
}

.list-info {
  display: flex;
}

.finishCheck {
  margin-right: 10px;
}

.finished {
  text-decoration: line-through;
  color: gray;
}
</style>
