<template>
    <div class="add-project">
        <form @submit.prevent="handleEdit">
            <label for="">Title</label>
            <input type="text" required v-model="this.title">
            <label for="">Details</label>
            <textarea v-model="this.details"></textarea>
            <button>edit project</button>
        </form>
    </div>
</template>

<script>
    export default {
        props: ['id'],
        data() {
            return {
                title: '',
                details: '',
                url: 'http://localhost:3000/projects/' + this.id
            }
        }, 
        mounted() {
            fetch(this.url)
                .then( res => res.json())
                .then( data => {
                    this.title = data.title
                    this.details = data.description
                })
                .catch( err => console.log(err.message))
        },
        methods: {
            handleEdit(){
                fetch(this.url, {
                    method: 'PATCH',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        title: this.title,
                        description: this.details
                    })
                }).then(() => {
                    this.$router.push({name: 'home'})
                }).catch(err => console.log(err))
            }
        }
    }
</script>

<style scoped>
    form {
        background: white;
        padding: 20px;
        border-radius: 10px;
    }
    label {
        display: block;
        color: #bbb;
        text-transform: uppercase;
        font-size: 14px;
        font-weight: bold;
        letter-spacing: 1px;
        margin: 20px 0 10px 0
    }
    input {
        padding: 10px;
        border: 0;
        border-bottom: 1px solid #ddd;
        width: 100%;
        box-sizing: border-box;
    }
    textarea {
        border: 1px solid #ddd;
        padding: 10px;
        width: 100%;
        box-sizing: border-box;
        height: 100px;
    }
    form button {
        display: block;
        margin: 20px auto 0;
        background: #00ce89;
        color: white;
        padding: 10px;
        border: 0;
        border-radius: 6px;
        font-size: 16px;
    }
</style>