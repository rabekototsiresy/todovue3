<template>
  <tr class="fw-normal px-2">
    <td class="align-middle">
      <span :class="[props.todo.isCompleted && 'todo-complted']">
        {{ props.todo.title }}
      </span>
    </td>
    <td class="align-middle">
      <span :class="[props.todo.isCompleted && 'todo-complted']">{{ props.todo.task }}</span>
    </td>

    <td class="text-end">
      <b-button
        @click="$bvModal.show('modal' + props.todo.id)"
        v-b-modal.modal-1
        class="btn btn-secondary"
        >Edit
      </b-button>
      <button
        :class="['btn  mx-2', props.todo.isCompleted ? 'btn-success' : 'btn-warning']"
        @click="toggleStateTodo"
        v-b-tooltip.hover :title="tooltipState"
      >
        <b-icon  v-if="props.todo.isCompleted" icon="check2-circle" aria-hidden="true"></b-icon>
        {{ props.todo.isCompleted ? 'Completed' : 'In progress' }}
      </button>
      

      <button class="btn btn-danger" @click="removeTodo">
        <b-icon icon="trash-fill"></b-icon>

        Delete
      </button>
    </td>
  </tr>

  <b-modal :id="'modal' + props.todo.id" title="Edit task" @ok="editTodo" ok-title="Update">
    <div>
      <label for="title">Title: </label>
      <input
        required
        type="text"
        class="form-control form-control-lg"
        id="title"
        placeholder="title..."
        v-model="todo.title"
      />
    </div>
    <div class="form-group mt-2">
      <label for="title">Content: </label>

      <textarea
        placeholder="Add new task"
        class="form-control"
        id="task"
        rows="3"
        v-model="todo.task"
        required
      ></textarea>
    </div>
  </b-modal>
</template>

<script setup>
import { ref, reactive, computed } from 'vue'

const props = defineProps({
  todo: { id: String, task: String }
})

const tooltipState = computed(() => props.todo.isCompleted ? 'Cick to switch to in progress' : ' Cick to switch to completed')
const todo = reactive({ task: '', title: '' })

todo.title = props.todo.title
todo.task = props.todo.task
const editTodo = () => {
  emit('edit', { id: props.todo.id, ...todo })
}
const removeTodo = () => {
  emit('remove', props.todo.id)
}

const toggleStateTodo = () => {
  emit('toggleState', props.todo.id)
}
const emit = defineEmits(['remove', 'edit', 'toggleState'])
</script>

<style scoped>
.todo-complted {
  text-decoration: line-through;
}
</style>
