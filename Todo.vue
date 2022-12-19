<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        class="col q-pr-md"
        filled
        square
        @keyup.enter="addTask"
        bg-color="white" 
        label="Task" 
        dense/>
      <q-input 
        v-model="taskImp" 
        class="col q-pr-xl"
        filled
        square
        side
        @keyup.enter="addTask"
        bg-color="white"
        label="Importance"
        placeholder="[0, 10]" 
        dense/>
      <q-btn 
        @click="addTask"
        flat 
        round
        dense
        side
        color="white" 
        icon="add" />
    </div>
    <q-list 
      class="bg-white"
      separator
      bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = !task.done"
        :class="{'done bg-indigo-1' : task.done}"
        clickable
        v-ripple>
        <q-item-section v-if="task.importance >= 8" avatar>
          <q-checkbox
            color="red"/>
        </q-item-section>
        <q-item-section v-else-if="task.importance >= 5" avatar>
          <q-checkbox
            color="orange"/>
        </q-item-section>
        <q-item-section v-else-if="task.importance >= 3" avatar>
          <q-checkbox
            color="amber"/>
        </q-item-section>
        <q-item-section v-else avatar>
          <q-checkbox
            color="yellow"/>
        </q-item-section>
        <q-item-section avatar>
          <q-checkbox 
            v-model="task.done"
            class="no-pointer-events"/>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section
          v-if="task.done"
          side>
            <q-btn 
              @click.stop="deleteTask(index)"
              flat 
              round
              dense
              color="primary" 
              icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div 
      v-if="!tasks.length"
      class="no-tasks absolute-center">
      <q-icon
        name = "done_all"
        size = "100px"
        color = "primary"/>
      <div class="text-h5 text-primary text-center">
        All done!
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  data() {
    return {
      newTask: '',
      tasks: [
        // {
        //   title: 'Eat sushi',
        //   done: false,
        //   importance: 1
        // },
        // {
        //   title: 'Sleep',
        //   done: false,
        //   importance: 4
        // },
        // {
        //   title: 'Master the jump serve',
        //   done: false,
        //   importance: 7
        // },
        // {
        //   title: 'Draw a realistic hand',
        //   done: false,
        //   importance: 10
        // }
      ]
    }
  },
  methods: {
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Are you sure you want to delete this task?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
        this.$q.notify('Task deleted successfully.')
      })
    },
    addTask() {
      this.tasks.push({
        title: this.newTask,
        done: false,
        importance: this.taskImp
      })
      this.newTask = ""
      this.taskImp = ""
    }
  }
})
</script>

<style lang="scss">
  .done {
    .q-item__label {
      text-decoration: line-through;
      color: #bbb;
    }
  }
  .no-tasks {
    opacity: 0.4;
  }
</style>