<template>
  <div class="container">
      <h3>Activity Log</h3>
      <button class="btn btn-info" v-on:click.prevent="toggleLogForm">{{logFormToggleText}}</button>
      <div id="form" v-if="showLogForm == true">
        <div v-if="showLogForm">
        <div id="addBookOption">
          <p>Not Seeing Your Book?</p>
          <router-link v-bind:to="{name : 'bookshelf'}" class="btn btn-info" tag="button">Add Book</router-link>
        </div>
          <add-reading-activity v-bind:currentUser="currentUser" v-on:add-log="toggleAndLoadLogs"/>
        </div>
      </div>
      <div id="activitySummary">
        <h4>Activity Summary</h4>
        <p>Number of Books Completed : {{completedBooks.length}}</p>
        <p>Reading Time : {{totalReadingTime}} minutes</p>
        <div v-if="currentBooks.length > 0" class="currentBooks">
          <p>Currently Reading : </p><p v-for="book in currentBooks" v-bind:key="book.bookId">{{book.title}}</p>
        </div>
        <div v-else class="currentBooks">
          <p>You are not reading any books right now</p>
        </div>
      </div>
      <h4>Recent Activity</h4>
      <div id="log-display-container">
        <div class="log" v-for="log in logs" v-bind:key="log.logId">
          <p><span class="grey">Book :</span> {{log.title}} </p>
          <p><span class="grey">Duration :</span> {{log.minutesSpent}} minutes</p>
          <p><span class="grey">Category :</span> {{log.readingFormat}} </p>
          <p><span class="grey">Notes :</span> {{log.bookNotes}} </p>
        </div>
        <div class="log" id="blank-log" v-if="logs.length % 2 == 1">
        </div>
      </div>
  </div>
</template>

<script>
import auth from '@/auth';
import AddReadingActivity from '@/components/AddReadingActivity.vue';
export default {
    props: {
      currentUser: Object
    },
  data() {
    return {
      logs: [],
      showLogForm: false,
      logFormToggleText: 'Add Activity',
      currentBooks: [],
      completedBooks: []
    }
  },
  components: {
    AddReadingActivity
  },
  methods: {
    getAllLogs() {
        fetch(`${process.env.VUE_APP_REMOTE_API}/api/activity/user/${this.currentUser.id}`, {
          method: 'GET',
            headers: {
            Accept: 'application/json',
            Authorization: 'Bearer ' + auth.getToken()
          }
          })
          .then((response) => {
            return response.json();
          })
          .then((data) => {
            this.logs = data;
          });
    },
    toggleLogForm() {
      this.showLogForm = !this.showLogForm;
      this.showLogForm ? this.logFormToggleText = 'Hide Form' : this.logFormToggleText = 'Add Activity';
    },
    toggleAndLoadLogs() {
      this.toggleLogForm();
      this.getAllLogs()
      this.getCurrentBooks();
      this.getCompletedBooks();
    },
    getCurrentBooks() {
       fetch(`${process.env.VUE_APP_REMOTE_API}/api/books/personalbookshelf/${this.currentUser.id}`, {
            method: 'GET',
              headers: {
              Accept: 'application/json',
              Authorization: 'Bearer ' + auth.getToken()
            }
            })
            .then((response) => {
              return response.json();
            })
            .then((data) => {
              this.currentBooks = data;
            });
    },
    getCompletedBooks() {
      fetch(`${process.env.VUE_APP_REMOTE_API}/api/books/completed/${this.currentUser.id}`, {
            method: 'GET',
              headers: {
              Accept: 'application/json',
              Authorization: 'Bearer ' + auth.getToken()
            }
            })
            .then((response) => {
              return response.json();
            })
            .then((data) => {
              this.completedBooks = data;
            });
    }
  },
  created() {
    this.getAllLogs()
    this.getCurrentBooks();
    this.getCompletedBooks();
  },
  computed: {
    totalReadingTime: function() {
      return this.logs.reduce((total, current) => 
        total + current.minutesSpent, 0
      )
    }
  } 
}
</script>

<style scoped>
p {
  margin: 0;
}
#log-display-container {
  display: flex;
  flex-wrap: wrap;
}
div.log {
    border: 2px solid lightgrey;
    border-radius: 10px;
    margin-top: 2%;
    padding: 5px;
    width: 45%;
    margin-left: auto;
    margin-right: auto;
}
span.grey {
  color: grey;
}
#form {
  margin-top: 2%;
  border: 1px solid grey;
  border-radius: 10px;
  padding: 10px;
  padding-bottom: 0;
  margin-bottom: 2%;
  width: 60%;
}
button.formSwitch {
  position: absolute;
  right: 0;
  margin-right: 42%;
}
div#activitySummary {
  margin-top: 3%;
  margin-bottom: 3%;
}
div.container {
  margin-bottom: 3%;
}
div#addBookOption {
  position: absolute;
  right: 0;
  margin-right: 42%;
}
div#blank-log {
  border: none;
}
</style>