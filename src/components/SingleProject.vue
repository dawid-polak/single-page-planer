<template>
    <div class="project" :class="{ complete: project.complete}">
        <div class="action">
            <h3 @click="this.showDetails = !this.showDetails">{{ project.title }}</h3>
            <div class="icons">
                <span class="material-icons" @click="deleteProject">delete</span>
                <span class="material-icons">edit</span>
                <span class="material-icons tick" @click="toggleComplite">done</span>
            </div>
        </div>
        <div v-if="showDetails" class="details">
            <p>{{ project.description }}</p>
        </div>
    </div>
</template>

<script>
    export default {
        name: 'SingleProject',
        props: ['project'],
        data() {
            return {
                showDetails: false,
                url: 'http://localhost:3000/projects/' + this.project.id
            }
        },
        methods: {
            deleteProject() {
                fetch(this.url, { method: 'DELETE' })
                    .then(() => this.$emit('delete', this.project.id))
                    .catch( err => console.log(err))
            },
            toggleComplite() {
                fetch(this.url, {
                    method: 'PATCH',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({ complete: !this.project.complete })
                }).then(() => {
                    this.$emit('complete', this.project.id)
                }).catch( err => console.log(err))
            }
        }
    }
</script>

<style scoped>
.project { 
        margin: 20px;
        padding: 10px 20px;
        border-radius: 4px;
        box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.05);
        border-left: 4px solid #e90074;
        background-color: #FFF;
    }

    h3 {
        cursor: pointer;
    }

    .action {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .details {
        text-align: left;
    }

    .material-icons {
        font-size: 24px;
        margin-left: 10px;
        color: #BBB;
        cursor: pointer;
    }

    .material-icons:hover {
        color: #777;
    }

    .project.complete {
        border-left: 4px solid #00ce89;
    }

    .project.project.complete .tick {
        color: #00ce89;
    }
</style>