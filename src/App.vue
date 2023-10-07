<template>
  <section class="vh-100">
    <div class="container p-3">
      <div class="row d-flex justify-content-center align-items-center h-100">
        <div class="col">
          <div class="card">
            <div class="card-body py-4 px-4 px-md-5">
              <p class="h2 text-center mt-3 mb-4 pb-3 text-primary d-flex justify-content-between">
                <u>
                  <b-icon icon="check2-circle" aria-hidden="true"></b-icon
                  ><span class="ml-2"> My Todos</span></u
                >
                <span> Todo in progress: {{ inProgressList }}</span>
              </p>

              <form class="pb-2" @submit.prevent="addTodo">
                <div class="card">
                  <div class="card-body">
                    <div class="form-group">
                      <input
                        required
                        type="text"
                        class="form-control form-control-lg"
                        id="title"
                        placeholder="Title todo..."
                        v-model="todo.title"
                      />
                    </div>
                    <div class="form-group mt-2">
                      <textarea
                        placeholder="Content todo"
                        class="form-control"
                        id="task"
                        rows="3"
                        v-model="todo.task"
                      ></textarea>
                    </div>

                    <div class="my-2">
                      <button
                        :disabled="!todo.title || !todo.task"
                        type="submit"
                        class="btn btn-primary"
                      >
                        <b-icon icon="plus-circle" aria-hidden="true"></b-icon>
                        Add
                      </button>
                    </div>
                  </div>
                </div>
              </form>

              <hr class="my-4" />

              <div class="overflow-auto">
                <table class="table mb-0">
                  <thead>
                    <tr class="p-2">
                      <th scope="col">Task title</th>
                      <th scope="col">Task Content</th>

                      <th scope="col" class="text-end">Actions</th>
                    </tr>
                  </thead>
                  <tbody>
                    <template v-for="todo in todoList" :key="todo.id">
                      <TodoItem
                        :todo="todo"
                        @edit="editTodo"
                        @remove="removeTodo"
                        @toggleState="toggleStateTodo"
                      />
                    </template>
                    <td v-if="todoList.length === 0" colspan="3" class="text-center p-2">
                      No task
                    </td>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import TodoItem from './components/TodoItem.vue'
import { v4 as uuidv4 } from 'uuid'
import { computed, reactive, ref } from 'vue'

const todo = reactive({ task: '', title: '' })
let todoList = ref([])
const inProgressList = computed(() => todoList.value.filter(todo =>!todo.isCompleted ).length)
/**
 * add todo with id from uuid library
 */
const addTodo = () => {
  console.log(todo)
  todoList.value.push({ id: uuidv4(), ...todo, isCompleted: false })
  todo.title = ''
  todo.task = ''
}
const removeTodo = (id) => {
  todoList.value = todoList.value.filter((todo) => todo.id !== id)
}

/**
 * Edit todo by id
 */
const editTodo = ({ task, title, id }) => {
  todoList.value = todoList.value.map((todo) => {
    if (todo.id === id)
      return {
        ...todo,
        task,
        title
      }
    return todo
  })
}

/**
 * Toggle state todo to completed or in progress
 */
const toggleStateTodo = (id) => {
  console.log('toogle id', id)
  todoList.value = todoList.value.map((todo) => {
    if (todo.id === id)
      return {
        ...todo,
        isCompleted: !todo.isCompleted
      }
    return todo
  })
}
</script>

<style scoped>
.card {
  border-radius: 0.75rem;
  background-color: #eff1f2;
}
</style>
