<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add a new task"
        dense
      >
        <template v-slot:append>
          <q-btn
            @click="addTask"
            round
            dense
            flat
            icon="add"
          />
        </template>
      </q-input>
    </div>
    <q-list
      class="bg-white"
      separator
      bordered
    >
      <q-item
        v-for="(task, index) in tasks"
        :key="index"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1' : task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            color="primary"
            class="no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section
          v-if="task.done"
          side
        >
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-task absolute-center">
      <q-icon
        name="check"
        size="100px"
        color="primary"
      />
      <div class="text-h5 text-primary text-center">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'TodoPage',
  data () {
    return {
      newTask : '',
      tasks: [
        // {
        //   title: 'Get groceries',
        //   done: false,
        // },
        // {
        //   title: 'Clean the house',
        //   done: false,
        // },
        // {
        //   title: 'Wash the car',
        //   done: false,
        // },
      ]
    }
  },
  methods: {
    addTask () {
      this.tasks.push({
        title: this.newTask,
        done: false,
      })
      this.newTask = ''
    },
    deleteTask (index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Are you sure you want to delete this task?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
        this.$q.notify({
          message: 'Task deleted successfully.',
          color: 'dark'
        })
      })
    },
  },
})
</script>

<style lang="scss">
  .done {
    .q-item__label {
      text-decoration: line-through;
      color: #bbb;
    }
  }
  .no-task {
    opacity: 0.5;
  }
</style>
