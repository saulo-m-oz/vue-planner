<template>
    <div class="project" :class="{ completed: project.completed }">
        <div class="actions">
            <h2 @click="showDetailsFn">{{project.title}}</h2>
            <div class="icons">
                <router-link :to="{name: 'EditProject', params: {id: project.id }}">
                    <span class="material-symbols-outlined">edit</span>
                </router-link>
                <span @click="deleteProject" class="material-symbols-outlined">delete</span>
                <span @click="finishProject" class="material-symbols-outlined tick">done</span>
            </div>
        </div>
        <Transition name="detail">
            <div v-if="showDetails">
                <div>{{project.details}}</div>
            </div>
        </Transition>
    </div>
</template>

<script>
export default {
props: ["project"],
data(){
    return{
        showDetails: false,
        uri: "http://localhost:3000/projects/" + this.project.id,
    }
},
methods: {
    showDetailsFn(){
        this.showDetails = !this.showDetails
    },
    deleteProject(){
        fetch(this.uri, {method: "DELETE"})
        .then( () => this.$emit("delete", this.project.id))
        .catch(err => console.error(err))
    },
    finishProject(){
        fetch(this.uri, 
        {method: "PATCH", 
        headers: {'Content-Type': 'application/json'},
        body: JSON.stringify({completed: !this.project.completed})
    })
        .then(()=> this.$emit("complete", this.project.id))
        .catch(err => console.error(err))
    }
}
}
</script>

<style>
    .project{
        margin: 20px auto;
        background: white;
        padding: 10px 20px;
        border-radius: 4px;
        box-shadow: 1px 2px 3px rgba(0,0,0,.15);
        border-left: 4px solid #c90074;
    }
    .project.completed{
        border-left: 4px solid #0ede42;
    }

    .completed .tick{
        color: #0ede42;
    }

    h2 {
        cursor: pointer;
    }

    .actions{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .material-symbols-outlined{
        font-size: 24px;
        margin-left: 10px;
        color: #bbb;
        cursor: pointer;
    }

    .material-symbols-outlined:hover{
        color: rgb(116, 116, 116);
    }

    .detail-enter-active{
        transition: all ease-in 0.15s;
    }

    .detail-leave-active{
        transition: all .25s ease-out;
    }

    .detail-enter-from,
    .detail-leave-to {
        opacity: 0;
    }
</style>