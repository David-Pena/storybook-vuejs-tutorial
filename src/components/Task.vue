<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { computed } from 'vue'

const props = defineProps({
  task: {
    type: Object,
    required: true,
    default: () => ({ id: '', state: '', title: '' }),
    validator: (task) => ['id', 'state', 'title'].every((key) => key in task)
  }
})

const emits = defineEmits(['archive-task', 'pin-task'])

const classes = computed(() => ({
  'list-item TASK_INBOX': props.task.state === 'TASK_INBOX',
  'list-item TASK_PINNED': props.task.state === 'TASK_PINNED',
  'list-item TASK_ARCHIVED': props.task.state === 'TASK_ARCHIVED'
}))

const isChecked = computed(() => props.task.state === 'TASK_ARCHIVED')

const archiveTask = () => {
  emits('archive-task', props.task.id)
}

const pinTask = () => {
  emits('pin-task', props.task.id)
}
</script>

<template>
  <div :class="classes">
    <label :for="'checked' + task.id" :aria-label="'archiveTask-' + task.id" class="checkbox">
      <input
        type="checkbox"
        :checked="isChecked"
        disabled
        :name="'checked' + task.id"
        :id="'archiveTask-' + task.id"
      />
      <span class="checkbox-custom" @click="archiveTask" />
    </label>
    <label :for="'title-' + task.id" :aria-label="task.title" class="title">
      <input
        type="text"
        readonly
        :value="task.title"
        :id="'title-' + task.id"
        name="title"
        placeholder="Input title"
      />
    </label>
    <button
      v-if="!isChecked"
      class="pin-button"
      @click="pinTask"
      :id="'pinTask-' + task.id"
      :aria-label="'pinTask-' + task.id"
    >
      <span class="icon-star" />
    </button>
  </div>
</template>
