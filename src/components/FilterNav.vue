<template>
  <div class="filter-nav">
    <button @click="updateFilter('all')" :class="{active: current === 'all'}">View All</button>
    <button @click="updateFilter('completed')" :class="{active: current === 'completed'}">Completed</button>
    <button @click="updateFilter('ongoing')" :class="{active: current === 'ongoing'}">Ongoing</button>
  </div>
</template>

<script>
export default {
    methods: {
        updateFilter(filter){
            this.$emit("filterChange", filter)
        }
    },
    props: ["current"]
}
</script>

<style>
    .filter-nav button{
        background: none;
        border: none;
        color: #bbb;
        outline: none;
        font-size: 12px;
        text-transform: uppercase;
        margin-right: 10px;
        letter-spacing: 1px;
        font-weight: bold;
        cursor: pointer;
    }
    .filter-nav button:hover{
        color: rgb(90, 90, 90);
    }
    .filter-nav button.active{
        color: rgb(90, 90, 90);
    }
</style>