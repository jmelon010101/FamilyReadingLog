<template>
  <div class="container">
      <h4>Child Activity Form</h4>
      <form v-on:submit.prevent="addActivity">
          <div class="form-group">
            <p>Reader</p>
                <select v-model="activity.userId" required>
                    <option v-for="child in children" v-bind:key="child.id" v-bind:value="child.id">
                        {{child.username}}
                    </option>
                </select>
          </div>
          <div class="form-group">
            <p>Book</p>
                <select v-model="activity.bookId" required>
                    <option v-for="book in books" v-bind:key="book.bookId" v-bind:value="book.bookId">
                        {{book.title}}
                    </option>
                </select>
          </div>
          <div class="form-group">
            <p>Form Of Reading</p>
                <select v-model="activity.readingFormat">
                    <option>Paper</option>
                    <option>Digital</option>
                    <option>Audiobook</option>
                    <option>Read-Aloud(Reader)</option>
                    <option>Read-Aloud(Listener)</option>
                    <option>Other</option>
                </select>
          </div>
          <div class="form-group">
            <label>Time Spent<b-form-input type="text" v-model="activity.minutesSpent" placeholder="minutes" required/></label>
          </div>
          <div class="form-group">
            <label>Notes<b-form-textarea v-model="activity.bookNotes"/></label>
          </div>
          <span>Did They Complete This Book?</span>
          <b-check v-model="activity.completedBook"></b-check>
          <b-button id="submitButton" type="submit">Add Child's Activity</b-button>
      </form>
  </div>
</template>

<script>
import auth from '@/auth';
export default {
    props: {
      currentUser: Object
    },
    data() {
        return {
            activity: {
                userId: '',
                bookId: '',
                readingFormat: '',
                minutesSpent: '',
                bookNotes: '',
                completedBook: false
            },
            books: [],
            children: []
        }
    },
    methods: {
        addActivity() {
           fetch(`${process.env.VUE_APP_REMOTE_API}/api/activity/`, {
                method: 'POST',
                headers: {
                    Accept: 'application/json',
                    'Content-Type': 'application/json',
                    Authorization: 'Bearer ' + auth.getToken()
                    },
                body: JSON.stringify(this.activity),
            })
            .then(() => this.$emit('add-log'));
            if (!this.activity.completedBook) {
            fetch(`${process.env.VUE_APP_REMOTE_API}/api/books/personalbookshelf/${this.activity.userId}/${this.activity.bookId}`, {
                method: 'POST',
                headers: {
                    Accept: 'application/json',
                    'Content-Type': 'application/json',
                    Authorization: 'Bearer ' + auth.getToken()
                    }
                }) 
            } else {
                fetch(`${process.env.VUE_APP_REMOTE_API}/api/books/personalbookshelf/${this.activity.userId}/${this.activity.bookId}`, {
                method: 'DELETE',
                headers: {
                    Accept: 'application/json',
                    'Content-Type': 'application/json',
                    Authorization: 'Bearer ' + auth.getToken()
                    }
                }) 
            }
        },
         getAllBooks() {
            fetch(`${process.env.VUE_APP_REMOTE_API}/api/books/${this.currentUser.familyId}`, {
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
                    this.books = data;
                });
            },
            getAllChildren() {
                fetch(`${process.env.VUE_APP_REMOTE_API}/api/users/child/${this.currentUser.familyId}`, {
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
                        this.children = data;
                    });
            }
    },
    created() {
        this.getAllBooks(),
        this.getAllChildren()
    }
}
</script>

<style scoped>
    label {
        width: 100%;
    }
    textarea {
        height: 100px;
    }
    p {
        margin: 0;
    }
    form {
        padding-bottom: 3%;
    }
    div.custom-checkbox {
        display: inline-block;
        margin-left: 1%;
        margin-bottom: 0;
    }
     #submitButton {
        display: block;
        margin-top: 4%;
    }
</style>