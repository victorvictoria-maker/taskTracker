<template>
  <button @click="addTask" :style="{background: color}" class="btn">{{text}}</button>
</template>

<script>
export default {
    name: 'ButtonComponent',
    props: {
        text: String,
        color: String
    },
    methods: {
        addTask () {
            this.$emit('btn-click');
        }
    },
}
</script>

<style>

</style>