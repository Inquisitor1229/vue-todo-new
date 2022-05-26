<template>
  <main>
    <div class="todo">
      <div class="todo__container">
        <div class="todo__header">
          <div class="todo__logo-container img-container">
            <img
                class="todo__logo"
                src="img/main-logo.svg"
                alt=""
                role="presentation"
                style="width: 180px; height: 180px;"
            >
          </div>
          <p class="todo__heading">Today I need to</p>
        </div>
        <div class="todo__body">
          <TaskInput @on-add-task="addTask"></TaskInput>
          <ul
              v-if="taskList.length > 0"
              class="task-list"
              role="list"
          >
            <li v-for="item in taskList" :key="item.id">
              <TaskCard
                  v-if="filter === 'all' || (filter === 'active' && !item.status) || (filter === 'completed' && item.status)"
                  :model="item"
                  @on-remove="removeTask(item.id)"
                  @on-done="doneTask(item.id)"
                  @on-edit="editTask"
              ></TaskCard>
            </li>
          </ul>
        </div>
        <div v-if="taskList.length === 0" class="todo__footer todo-footer">
          <div class="todo-footer__container">
            <div class="todo-footer__image img-container">
              <img
                  src="img/check.svg"
                  alt=""
                  role="presentation"
              >
            </div>
            <p class="todo-footer__description">Congrats, you have no more tasks to do</p>
          </div>
        </div>
        <div v-else>
          <task-progress :all-tasks = taskList>
          </task-progress>
          <task-filter
              :all-tasks="taskList"
              @on-check-all="checkAll"
              @on-filter-all="filterAll"
              @on-filter-active="filterActive"
              @on-filter-completed="filterCompleted"
              @on-clear-completed="clearCompleted"
          ></task-filter>
        </div>
      </div>
    </div>
    <test-ss></test-ss>
  </main>
</template>

<script>
    import TaskInput from "./components/task-input";
    import TaskCard from "./components/task-card";
    import TaskProgress from "./components/task-progress";
    import TaskFilter from "./components/task-filter";
    import {ref} from 'vue'

    export default {
        name: 'App',
        components: {
            TaskCard,
            TaskInput,
            TaskProgress,
            TaskFilter,
        },
        setup() {
            const taskList = ref([])
            let lastId = -1
            if (JSON.parse(localStorage.getItem('todo'))) {
                taskList.value = JSON.parse(localStorage.getItem('todo'))
                lastId = Number(localStorage.getItem('lastId'))
            }
            const addTask = ({title}) => {
                taskList.value = [...taskList.value, {id: ++lastId, title, status: false}]
                saveToLocalStorage()
            }
            const doneTask = (id) => {
                taskList.value = taskList.value.map(x => {
                    if (x.id === id)
                        if (x.status === false) x.status = true
                        else x.status = false
                    saveToLocalStorage()
                    return x
                })
            }
            const removeTask = (id) => {
                taskList.value = taskList.value.filter(x => x.id !== id)
                saveToLocalStorage()
            }
            const editTask = ({title, id}) => {
                taskList.value.map((x, index) => {
                    if (x.id === id) {
                        taskList.value.splice(index, 1, {id: x.id, title: title, status: x.status});
                    }
                })
                saveToLocalStorage()
            }
            const checkAll = () => {
                taskList.value.map((x) => {
                    if (!x.status)
                    {
                        doneTask(x.id)
                        x.status = true;
                    }
                })
                saveToLocalStorage()
            }
            let filter = ref('all')
            const filterAll = () => {
                filter.value = 'all';
            }
            const filterActive = () => {
                filter.value = 'active';
            }
            const filterCompleted = () => {
                filter.value = 'completed';
            }
            const clearCompleted = () => {
                taskList.value = taskList.value.filter(x => !x.status)

                saveToLocalStorage()
                localStorage.setItem('lastId', '-1');
            }
            const saveToLocalStorage = () => {
                localStorage.setItem('todo', JSON.stringify(taskList.value));
                localStorage.setItem('lastId', lastId.toString());
            }
            return {
                taskList,
                addTask,
                removeTask,
                doneTask,
                editTask,
                checkAll,
                filter,
                filterAll,
                filterActive,
                filterCompleted,
                clearCompleted,
            }
        },
    }
</script>