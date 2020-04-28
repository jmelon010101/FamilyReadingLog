<template>
  <div class="container">
      <form v-on:submit.prevent="addPrize">
          <div id="left-side-of-form">
            <div class="form-group">
                <label>Prize Name<b-form-input type="text" v-model="prize.prizeName" required/></label>
            </div>
            <div class="form-group">
                <label>Description<b-form-input type="text" v-model="prize.prizeDescription" required/></label>
            </div>
            <div class="form-group">
                <label>Minutes To Achieve It<b-form-input type="text" v-model="prize.minutesNeeded" required/></label>
            </div>
            <div class="form-group">
                <p>Who should the prize be available to?</p>
                    <select v-model="prize.userRole">
                        <option value="parent">Parent</option>
                        <option value="child">Child</option>
                        <option value="both">Both</option>
                    </select>
            </div>
          </div>
          <div id="right-side-of-form">
            <div class="form-group">
                <label>Max Number of Times It Can Be Given<b-form-input type="text" v-model="prize.maxPrizes" required/></label>
            </div>
            <div class="form-group">
                <label>Start Date<b-form-input type="date" v-model="prize.beginDate" required/></label>
            </div>
            <div class="form-group">
                <label>End Date<b-form-input type="date" v-model="prize.endDate" required/></label>
            </div>
            <button class="btn btn-info" type="submit">Add Prize</button>
          </div>
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
          prize: {
              familyId: this.currentUser.familyId,
              prizeName: '',
              prizeDescription: '',
              minutesNeeded: '',
              userRole: '',
              maxPrizes: '',
              beginDate: '',
              endDate: ''
          }
      }
  },
  methods: {
      addPrize() {
          fetch(`${process.env.VUE_APP_REMOTE_API}/api/prize/`, {
                method: 'POST',
                headers: {
                    Accept: 'application/json',
                    'Content-Type': 'application/json',
                    Authorization: 'Bearer ' + auth.getToken()
                    },
                body: JSON.stringify(this.prize),
            })
          .then(() => this.$emit('add-prize'));
      }
  }
}
</script>

<style scoped>
p {
    margin: 0;
}
form {
    display: flex;
    justify-content: space-between;
}
#left-side-of-form, #right-side-of-form{
    width: 45%;
    margin: 0;
}
</style>