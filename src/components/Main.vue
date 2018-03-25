<template lang="html">
  <div class="container">
    <div class="level">
      <div class="level-left">
        <div class="level-left">
          <div class="content">
            <h1>SpeedKamban</h1>
          </div>
        </div>
      </div>
      <div class="level-right">
        <div class="level-item ml-1">
          <button type="button" name="button" class="button is-info" v-on:click="postModal">Add New Task</button>
        </div>
      </div>
    </div>
    <div class="tile is-ancestor">
      <div class="tile is-parent is-3">
        <!-- first -->
        <article class="tile is-child">
          <div class="bordering">
            <div class="box violet">
              <div class="level">
                <div class="level-left">
                  <div class="content">
                    <h2 class="has-text-white has-text-weight-bold">Backlog</h2>
                  </div>
                </div>
              </div>
            </div>

            <div class="tasks notification is-danger" v-for="(backlog, index) in raw" :key="index">
              <div class="level">
                <div class="level-left onhover" v-on:click="detailModal(backlog)">
                  <div class="level-item">
                    <div class="content is-clipped">
                      <p class="has-text-white is-capitalized has-text-weight-semibold is-size-7">{{backlog.title}}</p>
                    </div>
                  </div>
                </div>
                <div class="level-right">
                  <div class="level-item ml-1">
                    <button v-show="backlog.status > 0" class="button is-small" type="button" name="back" v-on:click="unclear(backlog)">-</button>
                  </div>
                  <div class="level-item">
                    <button v-show="backlog.status < 3" class="button is-small" type="button" name="next" v-on:click="proceed(backlog)">+</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </article>
      </div>
      <div class="tile is-parent is-3">
        <!-- second -->
        <article class="tile is-child">
          <div class="bordering">
            <div class="box violet">
              <div class="level">
                <div class="level-left">
                  <div class="content">
                    <h2 class="has-text-white has-text-weight-bold">To-Do</h2>
                  </div>
                </div>
              </div>
            </div>

            <div class="tasks notification is-warning onhover" v-for="(todo, index) in fresh" :key="index">
              <div class="level">
                <div class="level-left onhover" v-on:click="detailModal(todo)">
                  <div class="level-item">
                    <div class="content is-clipped">
                      <p class="has-text-black is-capitalized has-text-weight-semibold is-size-7">{{todo.title}}</p>
                    </div>
                  </div>
                </div>
                <div class="level-right">
                  <div class="level-item ml-1">
                    <button v-show="todo.status > 0" class="button is-small" type="button" name="back" v-on:click="unclear(todo)">-</button>
                  </div>
                  <div class="level-item">
                    <button v-show="todo.status < 3" class="button is-small" type="button" name="next" v-on:click="proceed(todo)">+</button>
                  </div>
                </div>
              </div>
            </div>
          </div>

        </article>
      </div>
      <div class="tile is-parent is-3">
        <!-- third -->
        <article class="tile is-child">
          <div class="bordering">
            <div class="box violet">
              <div class="level">
                <div class="level-left">
                  <div class="content">
                    <h2 class="has-text-white has-text-weight-bold">Executing</h2>
                  </div>
                </div>
              </div>
            </div>

            <div class="tasks notification is-primary onhover" v-for="(doing, index) in splat" :key="index">
              <div class="level">
                <div class="level-left onhover" v-on:click="detailModal(doing)">
                  <div class="level-item">
                    <div class="content is-clipped">
                      <p class="has-text-white is-capitalized has-text-weight-semibold is-size-7">{{doing.title}}</p>
                    </div>
                  </div>
                </div>
                <div class="level-right">
                  <div class="level-item ml-1">
                    <button v-show="doing.status > 0" class="button is-small" type="button" name="back" v-on:click="unclear(doing)">-</button>
                  </div>
                  <div class="level-item">
                    <button v-show="doing.status < 3" class="button is-small" type="button" name="next" v-on:click="proceed(doing)">+</button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </article>
      </div>
      <div class="tile is-parent is-3">
        <!-- fourth -->
        <article class="tile is-child">
          <div class="bordering">
            <div class="box violet">
              <div class="level">
                <div class="level-left">
                  <div class="content">
                    <h2 class="has-text-white has-text-weight-bold">Completed</h2>
                  </div>
                </div>
              </div>
            </div>

            <div class="tasks notification is-success onhover" v-for="(finished, index) in cooked" :key="index">
              <div class="level">
                <div class="level-left onhover" v-on:click="detailModal(finished)">
                  <div class="level-item">
                    <div class="content is-clipped">
                      <p class="has-text-white is-capitalized has-text-weight-semibold is-size-7">{{finished.title}}</p>
                    </div>
                  </div>
                </div>
                <div class="level-right">
                  <div class="level-item ml-1">
                    <button v-show="finished.status > 0" class="button is-small" type="button" name="back" v-on:click="unclear(finished)">-</button>
                  </div>
                  <div class="level-item">
                    <button v-show="finished.status < 3" class="button is-small" type="button" name="next" v-on:click="proceed(finished)">+</button>
                  </div>
                </div>
              </div>
            </div>
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
          <ViewKanban :kanbanData="detailKanban" @closeModal="postModalOff" @updateTask="updateTaskData"></ViewKanban>
        </section>
        <footer class="modal-card-foot">
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
        status: null,
        key: null
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
      return this.kanbans.filter(task => task.status === 0)
    },
    fresh () {
      return this.kanbans.filter(task => task.status === 1)
    },
    splat () {
      return this.kanbans.filter(task => task.status === 2)
    },
    cooked () {
      return this.kanbans.filter(task => task.status === 3)
    }
  },
  methods: {
    submitKanban (newTask) {
      kanbanRef.push(newTask)
    },
    updateTaskData (update) {
      let data = {
        title: update.title,
        desc: update.desc,
        member: update.member,
        status: update.status
      }
      kanbanRef.child(update.key)
        .set(data)
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
      this.detailKanban.key = kanbanData['.key']
    },
    postModalOff () {
      this.postClass = 'modal'
      this.detailClass = 'modal'
    },
    proceed (task) {
      let update = task.status + 1
      kanbanRef.child(task['.key'])
        .child('status')
        .set(update)
    },
    unclear (task) {
      let update = task.status - 1
      kanbanRef.child(task['.key'])
        .child('status')
        .set(update)
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
  /* background-color: rgb(14, 223, 24); */
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
.violet {
 background-color: #f41664;
}
.jade {
  background-color: #44ed4a;
}
.ml-1 {
  margin-left: 1em;
}
</style>
