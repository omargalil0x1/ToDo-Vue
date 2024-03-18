<template>
  <div class="created-task-list">
    <div :id="'task-item' + '_' + single_task.id" v-for="single_task in taskList" class="card">
      <div class="card-body">
        <h5>{{ single_task.title }}</h5>
        <div class="card-body-text"style="height: 100%">
          {{ single_task.description }} - ID : {{ single_task.id }}
        </div>
        <div class="card-button-container">
          <button style="float: left" @click="emit_delete(single_task.id, single_task.title)" class="btn btn-danger"> Delete </button>

          <div v-if="!single_task.finished" class="task-status-container">
            <h4 style="margin-right: 4px"> Not Finished  </h4>
            <div class="spinner-border text-danger" role="status">
              <span class="visually-hidden">Loading...</span>
            </div>
          </div>

          <button style="float: right" @click="emit_finish(single_task.id, single_task.title)" class="btn btn-success">Finish</button>
        </div>

      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props: ['taskList'],

    methods: {
      emit_delete(task_id, task_title) {
        this.$emit('delete_task', task_id, task_title);
      },
      emit_finish(task_id, task_title) {
        this.$emit('finish_task', task_id, task_title);
      }
    }
  }
</script>
