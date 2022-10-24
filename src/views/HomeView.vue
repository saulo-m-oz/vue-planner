<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
      <div v-for="project in filteredProjects" :key="project.id">
        <TodoComponent @delete="handleDelete" @complete="handleComplete" :project="project"/>
      </div>
    </div> 
  </div>
</template>

<script>
import TodoComponent from "../components/TodoComponent.vue"
import FilterNav from "../components/FilterNav.vue"

export default {
  name: 'HomeView',
  data(){
    return{
      projects: [],
      current: "all"
    }
  },
  components: {
    TodoComponent,
    FilterNav
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
  },
  computed: {
    filteredProjects(){
      if (this.current === "completed") return this.projects.filter(project => project.completed)
      if (this.current === "ongoing") return this.projects.filter(project => !project.completed)
      return this.projects
    }
  }
}
</script>
