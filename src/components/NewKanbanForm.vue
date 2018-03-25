<template lang="html">
  <div>
    <div class="field">
      <label class="label">Title</label>
      <div class="control">
        <input v-model="newTask.title" type="text" class="input" name="title" placeholder="New task...">
        <p class="help is-danger">{{warningTitle}}</p>
      </div>
    </div>

    <div class="field">
      <label class="label">Description</label>
      <div class="control">
        <textarea v-model="newTask.desc" name="desc" class="textarea" placeholder="Describe the task here."></textarea>
        <p class="help is-danger">{{warningDesc}}</p>
      </div>
    </div>

    <div class="field">
      <label class="label">Member</label>
      <div class="control">
        <input v-model="newTask.member" type="text" class="input" name="member" placeholder="Assign someone here...">
        <p class="help is-danger">{{warningMember}}</p>
      </div>
    </div>

    <div class="field">
      <div class="control">
        <button class="button is-primary" v-on:click="submitTask">Submit</button>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'NewKanbanForm',
  data () {
    return {
      newTask: {
        title: '',
        desc: '',
        member: '',
        status: 0
      },
      warningTitle: '',
      warningDesc: '',
      warningMember: ''
    }
  },
  created () {
    this.warningTitle = ''
    this.warningDesc = ''
    this.warningMember = ''
  },
  methods: {
    submitTask () {
      let error = false
      if (this.newTask.title === '') {
        this.warningTitle = 'Must fill Title!'
        error = true
      }
      if (this.newTask.desc === '') {
        this.warningDesc = 'Please fill in the description'
        error = true
      }
      if (this.newTask.member === '') {
        this.warningMember = 'Must assign to someone'
        error = true
      }
      if (error !== true) {
        this.$emit('newTask', this.newTask)
        this.$emit('closeModal')
      }
    }
  }
}
</script>

<style lang="css">
</style>
