<template>
  <div class="task-input">
    <input
        v-model="title"
        class="task-input__title"
        placeholder="Add new todo..."
        type="text"
        @keyup.enter="onAddTask"
    >
    <button
        v-if="title"
        class="task-input__add"
        @click="onAddTask"
    >Submit</button>
  </div>
</template>

<script>
    import { ref } from 'vue'
    export default {
        emits: ['onAddTask',],
        setup(props, { emit }) {
            const title = ref('')
            const onAddTask = () => {
                if (!title.value) return false
                emit('onAddTask', { title: title.value })
                title.value = '';
            }
            return {
                title,
                onAddTask
            }
        }
    }
</script>