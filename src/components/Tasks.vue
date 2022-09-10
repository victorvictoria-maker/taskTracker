<template>
    <div :key="task.id" v-for="task in tasks">
        <EachTaskComponent :task="task" @delete-task="$emit('delete-task', task.id)" @toggle-reminder="$emit('toggle-reminder', task.id)"/>
    </div>
</template>


<script>
import EachTaskComponent from './EachTask.vue';

export default {
    name: 'TaskComponent',
    props: {
        tasks: Array 
    },
    components: {
        EachTaskComponent
    },
    emits: ['delete-task', 'toggle-reminder']
}
</script>


<style>

</style>