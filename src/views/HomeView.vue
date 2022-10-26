<template>
  <div class="home">
   <FilterProject @current="handleCurrent($event)" :current="this.current"/>
    <div v-if="projects" class="projects">
      <div v-for="project in filteredProjects" :key="project.id" >
        <SingleProject :project="project" @delete="handleDelete" @complete="handleComplite"/>
      </div>
    </div>


  </div>
</template>

<script>
import SingleProject from '../components/SingleProject.vue'
import FilterProject from '../components/FilterProject.vue'

export default {
  name: 'Home',
  components: {
   SingleProject,
   FilterProject
  },
  data() {
    return {
      projects: [],
      current: 'all'
    }
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter((project) => {
        return project.id !== id
      })
    },
    handleComplite(id){
      let p = this.projects.find( project => {
        return project.id === id
      })
      p.complete = !p.complete
    },
    handleCurrent(by) {
      this.current = by
    },
  },
  computed: {
    filteredProjects(){
      if (this.current === 'complete'){
        return this.projects.filter(project => project.complete)
      }
      if (this.current === 'ongoing'){
        return this.projects.filter(project => !project.complete)
      }
      return this.projects
    }
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then( res => res.json())
      .then ( data => this.projects = data)
      .catch( err => console.log(err.message))
  }
}
</script>

