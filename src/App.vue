<template>
  <Navbar>
    <template v-slot:create-task>
      <div class="main-form-container">
        <div class="main-form-title">
          <h2> Create Task </h2>
        </div>
        <hr>
        <div class="main-form-body">

          <div class="input-group mb-3">
            <span class="input-group-text" id="basic-addon1">What's task for today?</span>
            <input v-model="task_title" type="text" class="form-control" placeholder="Task Title" aria-label="Task Title" aria-describedby="basic-addon1">
          </div>
          <div class="input-group">
            <span class="input-group-text">Description</span>
            <textarea v-model="task_description" class="form-control" aria-label="Description" placeholder="Task Description"></textarea>
          </div>
          <br>
          <div class="text-center">
            <!-- div for holding the create task btn -->
            <button @click="printText" type="button" class="btn btn-primary">Create Task</button>
          </div>
        </div>
      </div>
    </template>
  </Navbar>
  <div class="taskList-analysis">

    <div class="block-101" style="width: 100%; overflow-y: auto; padding-right: 4px">
      <CreatedTasks @delete_task="deleteTask" @finish_task="finishTask" v-if="numberOfTasks > 0" :taskList="tasks_list"/>
      <FeedBack message="No Tasks Created Yet!" v-else/>
    </div>

    <div class="d-flex" style="height: 100%; color: white">
      <div class="vr"></div>
    </div>

    <div class="block-102" style="width: 100%; margin-right: 10px;">
      <UserAnalysis :tasksFinished="tasks_list_finished" :tasks_unfinished="tasks_list" />
    </div>

  </div>
  <FormValidationModal :title="modal_messages[0].title" :message="modal_messages[0].message"/>
</template>

<script>
  import $ from 'jquery'

  import Navbar from './components/Navbar.vue';

  import CreatedTasks from './components/CreatedTasks.vue';

  import UserAnalysis from './components/UserAnalysis.vue';

  import FeedBack from './components/FeedBack.vue';

  import FormValidationModal from './components/FormValidationModal.vue';

  export default {
    components: { Navbar, CreatedTasks, UserAnalysis, FeedBack, FormValidationModal },

    data: function() {
      return {
        task_title: null,
        task_description: null,
        numberOfTasks: 0,
        form_validated: true,
        tasks_list: [],

        tasks_list_finished: [],

        numberOfTasks_finished: 0,

        modal_messages: [
          {title: null, message: null}
        ],

      }
    },
    methods: {

      pushElement() {

        // start pushing the element itself.
        this.tasks_list.push({
          id: this.numberOfTasks, title: this.task_title, description: this.task_description,
          finished: false,
        });

        this.numberOfTasks += 1;
      },

      printText() {

        let modal_active = document.getElementById('modal_activation');

        if(this.task_title == null || this.task_description == null) {

            this.form_validated = false;

            this.modal_messages[0].title = "Error"

            this.modal_messages[0].message = "Empty Title/Description of Title"

            modal_active.click()

        } else {

          if(this.tasks_list.length === 0) {

            this.pushElement()


          } else {

            for(let i = 0; i < this.tasks_list.length; i++) {
              if(this.tasks_list[i].title == this.task_title) {
                this.modal_messages[0].title = "Duplicated Tasks"
                this.modal_messages[0].message = "Task Already Created"
                modal_active.click()
                break
              } else {
                this.pushElement()
                console.log(this.tasks_list.length)

                break
              }
            }
          }
        }
      },

      deleteTask(task_id, task_title) {

        for(let i = 0; i < this.tasks_list.length; i++) {
          if(this.tasks_list[i].title == task_title) {

            this.tasks_list.splice(i, 1);

            this.numberOfTasks -= 1;
            break;
          }
        }

      },

      finishTask(task_id, task_title) {
        this.tasks_list[task_id].finished = true; // mark the task as finished.

        this.tasks_list_finished.push(this.tasks_list[task_id]); // push that element to the array of finished tasks.

        this.deleteTask(task_id, task_title)

        this.numberOfTasks_finished += 1; // increase the number of finished tasks.

      }
    }
  }

</script>
