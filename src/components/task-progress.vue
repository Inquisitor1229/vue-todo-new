<template>
  <div class="task-progress">
    <div class="task-progress__container">
      <div class="task-progress__card">
        <div class="task-progress__card-container">
          <p class="task-progress__card-count">{{completedTasks}} tasks</p>
          <p class="task-progress__card-heading">Completed</p>
          <div class="task-progress__card-progressbar progressbar">
            <div class="progressbar__value progressbar__value--completed" :style="{width: ((completedTasks / allTasks.length) * 100) + '%'}"></div>
          </div>
        </div>
      </div>
      <div class="task-progress__card">
        <div class="task-progress__card-container">
          <p class="task-progress__card-count">{{allTasks.length - completedTasks}} tasks</p>
          <p class="task-progress__card-heading">To be finished</p>
          <div class="task-progress__card-progressbar progressbar">
            <div class="progressbar__value progressbar__value--not-completed" :style="{ width: (((allTasks.length - completedTasks) / allTasks.length) * 100) + '%'}"></div>
          </div>
        </div>
      </div>
    </div>
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
        computed: {
            completedTasks() {
                let count = 0;
                this.allTasks.map((x) => {
                    if (x.status) count++
                })
                return count
            }
        }
    }
</script>