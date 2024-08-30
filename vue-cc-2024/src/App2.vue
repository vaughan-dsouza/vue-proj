<!-- <script >
export default {
  data(){
    return {
      name: 'john doe',
      status: 'active',
    };
  },

  methods:{
    togglestatus() {
      if (this.status === 'active'){
        this.status = 'pending';
      } else if (this.status === 'pending'){
        this.status = 'inactive';
      } else {
        this.status = 'active';
      }
    }
  }
};
</script> -->

<script setup>
import { ref, onMounted } from 'vue';

const name = ref('john doe');
const status = ref('active');
const newTask = ref('newTask')
const tasks = ref(['task one', 'task two']);

const togglestatus = () => {
  if (status.value === 'active'){
    status.value = 'pending';
  } else if (status.value === 'pending'){
    status.value = 'inactive';
  } else {
    status.value = 'active';
  }
};

const addTask = () =>{
  if(newTask.value.trim() !== ''){
    tasks.value.push(newTask.value);
    newTask.value = '';
  }
}

const deleteTask = (inx) =>{
  tasks.value.splice(inx , 1);
}

onMounted(async () => {
  try{
    const response= await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  }catch(error){
    console.log('error fetching task');
  }
});
</script>


<template>
  <h1>{{ name }}</h1>
  <span v-if="status === 'active'">User active</span>
  <span v-else-if="status === 'pending'">User pending</span>
  <span v-else>User inactive</span>
  <br  />
  <button @click="togglestatus">change status</button>

  <form @submit.prevent="addTask">
    <label for="newTask">add Task</label>
    <input type="text" id="newTask" class="newTask" v-model="newTask" /> 
    <button type="submit">ADD</button>
  </form>

  <span>Tasks</span>
  <ol>
      <li v-for="(task, index) in tasks" :key="task">
        <span>
          {{ task }}
        </span>
        <button @click="deleteTask(index)">x</button>
        </li>
  </ol>
</template>

<style scoped>

</style>
