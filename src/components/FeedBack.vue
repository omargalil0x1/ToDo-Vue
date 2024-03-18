<template>
  <div style="margin-right: 0px;" class="no-tasks-feedback">
    <div class="alert alert-danger" role="alert">
      {{ message }}
    </div>
  </div>
</template>


<script>
  export default {
    props: ['message'],
  }
</script>
