<template>
  <div class="home">
    <h1>Todos</h1>
    <div v-if="projects.length">
      <div v-for="project in projects" :key="project.id">
        <TodoComponent @delete="handleDelete" @complete="handleComplete" :project="project"/>
      </div>
    </div> 
  </div>
</template>

<script>
import TodoComponent from "../components/TodoComponent.vue"

export default {
  name: 'HomeView',
  data(){
    return{
      projects: []
    }
  },
  components: {
    TodoComponent
  },
  mounted(){
    fetch("http://localhost:3000/projects")
    .then(res => res.json())
    .then(data => this.projects = data)
    .catch(err => console.error(err.message))
  },
  methods: {
    handleDelete(projectID){
     this.projects = this.projects.filter(project => project.id !== projectID)
    },
    handleComplete(projectID){
      const project = this.projects.find(project => project.id === projectID)
      project.completed = !project.completed;
    }
  }
}
</script>
