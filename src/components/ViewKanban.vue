<template lang="html">
  <div class="">
    <div>

      <section :class="heroclass">
        <div class="hero-body">
          <div class="has-text-left">
            <h1 class="title is-capitalized">
              {{kanbanData.title}}
            </h1>
            <h2 class="subtitle">
              {{kanbanData.member}}
            </h2>
            <div class="content">
              <p>{{kanbanData.desc}}</p>
            </div>
          </div>
        </div>
      </section>
      <section class="section has-text-left">
        <div class="content">
          <h1>Update data</h1>
        </div>
        <div class="container is-fluid">
          <div class="field">
            <label class="label">Title</label>
            <div class="control">
              <input v-model="currentTask.title" type="text" class="input" name="title">
              <p class="help is-danger">{{warningTitle}}</p>
            </div>
          </div>

          <div class="field">
            <label class="label">Description</label>
            <div class="control">
              <textarea v-model="currentTask.desc" name="desc" class="textarea"></textarea>
              <p class="help is-danger">{{warningDesc}}</p>
            </div>
          </div>

          <div class="field">
            <label class="label">Member</label>
            <div class="control">
              <input v-model="currentTask.member" type="text" class="input" name="member">
              <p class="help is-danger">{{warningMember}}</p>
            </div>
          </div>

          <div class="field">
            <div class="control">
              <button class="button is-primary" v-on:click="updateTask">Submit</button>
            </div>
          </div>
        </div>
      </section>

    </div>
  </div>
</template>

<script>
export default {
  name: 'ViewKanban',
  props: ['kanbanData'],
  data () {
    return {
      currentTask: {
        title: '',
        desc: '',
        member: '',
        status: '',
        key: ''
      },
      warningTitle: '',
      warningDesc: '',
      warningMember: '',
      heroclass: 'hero'
    }
  },
  created () {
    // console.log(this.kanbanData)
    this.currentTask.title = ''
    this.currentTask.desc = ''
    this.currentTask.member = ''
    this.warningTitle = ''
    this.warningDesc = ''
    this.warningMember = ''
  },
  methods: {
    updateTask () {
      let error = false
      if (this.currentTask.title === '') {
        this.warningTitle = 'Must fill Title!'
        error = true
      }
      if (this.currentTask.desc === '') {
        this.warningDesc = 'Please fill in the description'
        error = true
      }
      if (this.currentTask.member === '') {
        this.warningMember = 'Must assign to someone'
        error = true
      }
      if (error !== true) {
        this.currentTask.key = this.kanbanData.key
        // console.log(this.currentTask)
        this.$emit('updateTask', this.currentTask)
        this.$emit('closeModal')
      }
    }
  },
  computed: {
    currentStatus () {
      return this.kanbanData.status
    },
    currentTitle () {
      return this.kanbanData.title
    },
    currentDesc () {
      return this.kanbanData.desc
    },
    currentMember () {
      return this.kanbanData.member
    }
  },
  watch: {
    currentStatus: function (newVal, oldVal) {
      if (newVal === 0) {
        this.heroclass = 'hero is-danger is-bold'
      } else if (newVal === 1) {
        this.heroclass = 'hero is-warning is-bold'
      } else if (newVal === 2) {
        this.heroclass = 'hero is-info is-bold'
      } else if (newVal === 3) {
        this.heroclass = 'hero is-success is-bold'
      }
      this.currentTask.status = newVal
    },
    currentTitle: function (newVal, oldVal) {
      this.currentTask.title = newVal
    },
    currentDesc: function (newVal, oldVal) {
      this.currentTask.desc = newVal
    },
    currentMember: function (newVal, oldVal) {
      this.currentTask.member = newVal
    }
  }
}
</script>

<style lang="css">
.w-100 {
  width: 100%;
}
</style>
