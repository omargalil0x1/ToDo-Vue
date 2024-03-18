<template>
  <div class="card text-center user-analysis" style="height: 100%">
    <div class="card-header">
      <ul class="nav nav-tabs card-header-tabs">
        <li class="nav-item">
          <a class="nav-link" :class="{active: !switched}" aria-current="true" @click="clickHandler($event, 'first')">General Information</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" :class="{active: switched}" @click="clickHandler($event, 'second')">
            Finished Tasks <span class="badge text-bg-danger"> {{ tasksFinished.length >= 100 ? "+99" : tasksFinished.length }} </span>
          </a>
        </li>
      </ul>
    </div>

    <div v-if="!switched" class="card-body" :class="{active: !switched}">
      <h5 class="card-title"> Information </h5>

      <div class="general-information">

        <div class="unfinished_tasks">
          Unfinished Tasks : {{ tasks_unfinished.length > 0 ? tasks_unfinished.length : 0 }}
        </div>
        <div class="finished_tasks">
          Finished Tasks : {{ tasksFinished.length > 0 ? tasksFinished.length : 0 }}
        </div>
      </div>

    </div>
    <div class="card-body" v-else :class="{active: switched}">
      <h5 class="card-title"> Finished Tasks </h5>
      <table v-if="tasksFinished.length > 0" class="table">
        <thead>
          <tr>
            <th scope="col">Task Title</th>
            <th scope="col">Task Description</th>
          </tr>
        </thead>
        <tbody>
          <tr class="table-info" v-for="single_finished_task in tasksFinished">
            <th> {{single_finished_task.title}} </th>
            <td> {{single_finished_task.description}} </td>
          </tr>
        </tbody>
      </table>

      <div v-else class="feedback-finished-tasks">
        <span style="width: 100%"> No Finished Tasks... </span> <br>
        <button style="width: 100%" class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
          Start Creating One
        </button>
      </div>

    </div>
  </div>
</template>

<script>
  export default {
    props: ['tasksFinished', 'tasks_unfinished'],

    data: function() {
      return {
        switched: false,
      }
    },

    methods: {
      clickHandler(event, target) {
        if(target == "first") {
          this.switched = false;
          if(switched == true) {
            this.switched = false;
          }
        } else if(target == "second") {
          this.switched = true;
        }
      }
    }
  }
</script>
