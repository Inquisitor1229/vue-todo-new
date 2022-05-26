<template>
  <div class="task-card">
    <div class="task-card__move-element">
      <img
          src="img/card-move.svg"
          alt="Move element"
          class="task-card__move-element"
      >
    </div>
    <input
        :checked="model.status"
        type="checkbox"
        class="task-card__status"
        @change="emitOnDone"
    >
    <div v-if="editStatus">
      <input
          v-model="title"
          class="task-input__title"
          placeholder="Add new todo..."
          type="text"
          @keyup.enter="editTask"
      >
    </div>
    <div v-else>
      <p class="task-card__text" :class="{ 'task-card__text--done': model.status }">{{ model.title }}</p>
    </div>
    <div class="task-card__options">
      <button v-if="!editStatus || title" class="task-card__options-button img-container">
        <img
            src="img/edit.svg"
            alt="edit"
            @click="editTask"
        >
      </button>
      <button class="task-card__options-button img-container" @click="emitOnRemove">
        <img
            src="img/delete.svg"
            alt="delete"
        >
      </button>
    </div>
  </div>
</template>

<script>
    import {ref} from "vue";

    export default {
        props: {
            model: {
                required: true,
                type: Object,
                default() {
                    return {
                        id: Number,
                        title: String,
                        status: ref(Boolean)
                    }
                }
            }
        },
        emits: ['onDone', 'onRemove', 'onEdit',],
        setup(props, {emit}) {
            const title = ref('')
            const id = ref('')
            const emitOnDone = () => {
                emit('onDone')
            }
            const emitOnRemove = () => {
                emit('onRemove')
            }
            const onChangeTask = () => {
                if (title.value) {
                    emit('onEdit', {title: title.value, id: id.value})
                    title.value = '';
                }
            }
            return {
                title,
                id,
                emitOnDone,
                emitOnRemove,
                onChangeTask,
            }
        },
        data() {
            return {
                editStatus: false,
                taskStatus: false,
            }
        },
        methods: {
            editTask() {
                if (!this.editStatus) {
                    this.title = this.model.title;
                    this.id = this.model.id;
                } else {
                    this.onChangeTask()
                }
                this.editStatus = !this.editStatus;
            }
        }
    }
</script>