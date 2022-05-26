<template>
  <div class="task-filter">
    <button
        class="task-filter__item"
        :style="{ visibility: (taskTypes === 'activeAndCompleted' || taskTypes === 'allActive') ? 'visible' : 'hidden'}"
        @click="onCheckAll"
    >Check all</button>
    <input
        id="filter_all"
        name="todo_filter"
        type="radio"
        class="task-filter__radio visually-hidden"
        checked
    >
    <label
        for="filter_all"
        class="task-filter__item"
        :style="{ visibility: (taskTypes === 'activeAndCompleted' || taskTypes === 'allActive' || taskTypes === 'allCompleted') ? 'visible' : 'hidden'}"
        @click="onFilterAll"
    >All</label>
    <input
        id="filter_active"
        name="todo_filter"
        type="radio"
        class="task-filter__radio visually-hidden"
    >
    <label
        for="filter_active"
        class="task-filter__item"
        :style="{ visibility: (taskTypes === 'activeAndCompleted') ? 'visible' : 'hidden'}"
        @click="onFilterActive"
    >
      Active
    </label>
    <input
        id="filter_completed"
        name="todo_filter"
        type="radio"
        class="task-filter__radio visually-hidden"
    >
    <label
        for="filter_completed"
        class="task-filter__item"
        :style="{ visibility: (taskTypes === 'activeAndCompleted') ? 'visible' : 'hidden'}"
        @click="onFilterCompleted"
    >Completed</label>
    <button
        class="task-filter__item"
        :style="{ visibility: (taskTypes === 'activeAndCompleted' || taskTypes === 'allCompleted') ? 'visible' : 'hidden'}"
        @click="onClearCompleted"
    >Clear completed</button>
  </div>
</template>

<script>
    export default {
        props: {
            allTasks: {
                required: true,
                type: Array,
            }
        },
        emits: [
            'onCheckAll',
            'onFilterAll',
            'onFilterActive',
            'onFilterCompleted',
            'onClearCompleted',
        ],
        setup(props, {emit}) {
            const onCheckAll = () => {
                emit('onCheckAll')
            }
            const onFilterAll = () => {
                emit('onFilterAll')
            }
            const onFilterActive = () => {
                emit('onFilterActive')
            }
            const onFilterCompleted = () => {
                emit('onFilterCompleted')
            }
            const onClearCompleted = () => {
                emit('onClearCompleted')
            }
            return {
                onCheckAll,
                onFilterAll,
                onFilterActive,
                onFilterCompleted,
                onClearCompleted,
            }
        },
        computed: {
            taskTypes() {
                let count = 0;
                this.allTasks.map((x) => {
                    if (x.status) count++
                })
                if (count === 0) {
                    return 'allActive'
                } else if (count === this.allTasks.length) {
                    return 'allCompleted'
                } else {
                    return 'activeAndCompleted'
                }
            }
        }
    }
</script>