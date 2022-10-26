<template>
    <div class="filter-project">
        <button 
        @click="changeCurrentValue('all')" 
        :class="{ active: current === 'all'}"
        >All projects</button>
        <button 
        @click="changeCurrentValue('complete')" 
        :class="{ active: current === 'complete'}"
        >Complite</button>
        <button 
        @click="changeCurrentValue('ongoing')" 
        :class="{ active: current === 'ongoing'}"
        >On going</button>
    </div>
</template>

<script>
    export default {
        props: ['current'],
        methods: {
            changeCurrentValue(by) {
                this.$emit('current', by)
            }
        }
    }
</script>

<style scoped>
 .filter-project button {
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
  .filter-project button.active {
    color: #555;
  }
</style>