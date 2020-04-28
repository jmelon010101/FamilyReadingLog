<template>
    <div class="container outer-container">
        <h3>Prizes</h3>
        <div id="top-image">
            <img id="prizeImage" src='@/assets/images/prizeheader.jpg'/>
        </div>
        <h5>Your Total Available Minutes: X</h5>
        <button class="btn btn-info" v-on:click="togglePrizeForm"> {{formButtonText}} </button>
        <div id="prizeForm" v-if="showPrizeForm">
          <add-prize v-on:add-prize="toggleFormAndLoadPrizes" v-bind:currentUser="currentUser"/>
        </div>
        <div class="prizeCards">
            <div class="prize" v-for="prize in prizes" v-bind:key="prize.prizeId">
              <div class="trophy">
                <img class="trophyImg" src="@/assets/images/trophyicon.jpg"/>
              </div>
              <div class="title">
                <span class ="bold"> {{prize.prizeName}} </span>
              </div>
              <div class="prizeContent">
                  <p>User Group: {{prize.userRole}}</p>
                  <p>Start Date: {{prize.beginDate}}</p>
                  <p>End Date: {{prize.endDate}}</p>
                  <span class="bold">Minutes Needed: {{prize.minutesNeeded}}</span>
              </div>
              <div class="buttons">
                <div class="select">
                  <button class="btn btn-info">Choose Prize/Apply Minutes</button>
                </div>
                <div class="editPrize">
                  <button class="btn btn-info" >Edit/Delete Prize</button>
                </div>
              </div>
            </div>
        </div>
    </div>
  
</template>

<script>
import auth from '@/auth';
import AddPrize from '@/components/AddPrize.vue';
export default {
  props: {
    currentUser: Object
  },
  components: {
    AddPrize
  },
  data(){
      return{
        showPrizeForm: false,
        formButtonText: 'Add a New Prize!',
        prizes: []
      }
  },
  methods: {
    togglePrizeForm() {
      this.showPrizeForm = !this.showPrizeForm;
      this.showPrizeForm ? this.formButtonText = 'Hide Form' : this.formButtonText = 'Add a New Prize!';
    },
    toggleFormAndLoadPrizes() {
      this.togglePrizeForm();
      this.loadAllFamilyPrizes();
    },
    loadAllFamilyPrizes() {
       fetch(`${process.env.VUE_APP_REMOTE_API}/api/prizes/${this.currentUser.familyId}`, {
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
          this.prizes = data;
        });
    }
  },
  created() {
    this.loadAllFamilyPrizes();
  }

}
</script>

<style scoped>

  div#top-image {
    max-height: 200px; 
    /* make responsive */
    overflow: hidden;
    border-radius: 15px;
    margin-bottom: 2%;
  }

  #prizeImage {
    width: 91vw;
    height: auto;
       /* make responsive */
  }

  .bold{
    font-weight: bold;
  }

  .prizeCards{
    display: flex;
    flex: row;
    flex-wrap: wrap;
    width: 91 vw;
  }

  div.prize{
    border: 2px solid lightgrey;
    border-radius: 5px;
    margin: 1%;
    padding: 10px;
    width: 30%;
    max-height: 50%;
  }

  .trophyImg{
    width: 20%;
  }

  .select, .editPrize{
    margin: 5%;
  }

  div#prizeForm {
    margin-top: 2%;
    border: 1px solid grey;
    width: 80%;
    border-radius: 15px;
    padding: 10px;
  }

  div.outer-container {
    margin-bottom: 3%;
  }
</style>