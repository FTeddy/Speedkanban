<template lang="html">
  <div class="container">
    <div class="tile is-ancestor">
      <div class="tile is-parent">
        <article class="tile is-child bordering">
          <div class="box">
            <div class="content">
              <h2>Raw</h2>
            </div>
            <button type="button" name="button" class="button is-info" v-on:click="postModal">post</button>
          </div>

          <div class="tasks notification is-primary onhover" v-for="(backlog, index) in raw" :key="index" v-on:click="detailModal(backlog)">
            <div class="level">
              <div class="level-left">
                <div class="level-item">
                  <div class="content">
                    <h2 class="has-text-white is-capitalized has-text-weight-semibold">{{backlog.title}}</h2>
                  </div>
                </div>
              </div>
              <div class="level-right">
                <div class="level-item">
                  <button class="button" type="button" name="back" v-on:click="unclear">-</button>
                </div>
                <div class="level-item">
                  <button class="button" type="button" name="next" v-on:click="proceed">+</button>
                </div>
              </div>
            </div>
          </div>
        </article>
      </div>
      <div class="tile is-parent">
        <article class="tile is-child">
          <div class="box">
            <div class="content">
              <h2>Fresh</h2>
            </div>
            <button type="button" name="button" class="button is-info" v-on:click="postModal">post</button>
          </div>

          <div class="content notification is-primary" v-for="(todo, index) in fresh" :key="index">
            <h4 class="has-text-white">{{todo.title}}</h4>
            <button type="button" name="button" class="button is-info" v-on:click="detailModal">info</button>
          </div>
        </article>
      </div>
      <div class="tile is-parent">
        <article class="tile is-child">
          <div class="box">
            <div class="content">
              <h2>Splat</h2>
            </div>
            <button type="button" name="button" class="button is-info" v-on:click="postModal">post</button>
          </div>

          <div class="content notification is-primary" v-for="(doing, index) in splat" :key="index">
            <h4 class="has-text-white">{{doing.title}}</h4>
            <button type="button" name="button" class="button is-info" v-on:click="detailModal">info</button>
          </div>
        </article>
      </div>
      <div class="tile is-parent">
        <article class="tile is-child">
          <div class="box">
            <div class="content">
              <h2>Cooked</h2>
            </div>
            <button type="button" name="button" class="button is-info" v-on:click="postModal">post</button>
          </div>

          <div class="content notification is-primary" v-for="(finished, index) in cooked" :key="index">
            <h4 class="has-text-white">{{finished.title}}</h4>
            <button type="button" name="button" class="button is-info" v-on:click="detailModal">info</button>
          </div>
        </article>
      </div>
    </div>

    <div :class="postClass">
      <div class="modal-background" v-on:click="postModalOff"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">New Task</p>
          <button class="delete" aria-label="close" v-on:click="postModalOff"></button>
        </header>
        <section class="modal-card-body">
          <!-- Content ... -->
          <NewKanbanForm @newTask="submitKanban" @closeModal="postModalOff"></NewKanbanForm>
        </section>
        <footer class="modal-card-foot">
          <button class="button is-success">Save changes</button>
          <button class="button" v-on:click="postModalOff">Cancel</button>
        </footer>
      </div>
    </div>

    <div :class="detailClass">
      <div class="modal-background" v-on:click="postModalOff"></div>
      <div class="modal-card">
        <header class="modal-card-head">
          <p class="modal-card-title">Task Details</p>
          <button class="delete" aria-label="close" v-on:click="postModalOff"></button>
        </header>
        <section class="modal-card-body">
          <!-- Content ... -->
          <ViewKanban :kanbanData="detailKanban"></ViewKanban>
        </section>
        <footer class="modal-card-foot">
          <button class="button is-success">Save changes</button>
          <button class="button" v-on:click="postModalOff">Cancel</button>
        </footer>
      </div>
    </div>
  </div>
</template>

<script>
import * as firebase from 'firebase'
import ViewKanban from '@/components/ViewKanban'
import NewKanbanForm from '@/components/NewKanbanForm'
const config = {
  apiKey: 'AIzaSyApg6pnCCpqDYYg2-HJWDsr1nF3i3p3RjQ',
  databaseURL: 'https://hacktiv8-2c76e.firebaseio.com',
  projectId: 'hacktiv8-2c76e'
}
const fireApp = firebase.initializeApp(config)
const db = fireApp.database()
const kanbanRef = db.ref('kanbans')
export default {
  name: 'Main',
  components: {
    ViewKanban,
    NewKanbanForm
  },
  firebase: {
    kanbans: kanbanRef
  },
  data () {
    return {
      detailKanban: {
        title: null,
        desc: null,
        member: null,
        status: null
      },
      postClass: 'modal',
      detailClass: 'modal'
    }
  },
  created () {
    this.postClass = 'modal'
    this.detailClass = 'modal'
  },
  computed: {
    raw () {
      return this.kanbans.filter(task => task.status === 'Raw')
    },
    fresh () {
      return this.kanbans.filter(task => task.status === 'fresh')
    },
    splat () {
      return this.kanbans.filter(task => task.status === 'splat')
    },
    cooked () {
      return this.kanbans.filter(task => task.status === 'cooked')
    }
  },
  methods: {
    submitKanban (newTask) {
      console.log(newTask)
      kanbanRef.push(newTask)
      console.log(this.raw)
    },
    postModal () {
      this.postClass = 'modal is-active'
    },
    detailModal (kanbanData) {
      this.detailClass = 'modal is-active'
      this.detailKanban.title = kanbanData.title
      this.detailKanban.desc = kanbanData.desc
      this.detailKanban.member = kanbanData.member
      this.detailKanban.status = kanbanData.status
    },
    postModalOff () {
      this.postClass = 'modal'
      this.detailClass = 'modal'
      this.detailKanban.title = ''
      this.detailKanban.desc = ''
      this.detailKanban.member = ''
      this.detailKanban.status = ''
    },
    proceed () {
      console.log('proceeding')
    },
    unclear () {
      console.log('going back...')
    }
  }

}
</script>

<style lang="css">
.notification {
  padding: 0.5em 0;
  border-radius: 6px;
}
.notification:not(:last-child) {
    margin-bottom: 1rem;
}
.content h2 {
  margin: 0;
}
.box {
  background-color: rgb(14, 223, 24);
}
.onhover {
  cursor: pointer;
}
.tasks {
  padding: 0.5em 0.8em;
}
.bordering {
  border-radius: 10px;
  border: 2px solid rgb(150, 150, 255);
  padding: 4px;
}
</style>
