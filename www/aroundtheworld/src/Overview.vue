<template>
  <div class="overview">
    <div class="overview-header">
      <p class="overview-title">{{title}}</p>

      <nav class="detail-nav">
        <a id="overview" href='#'> Overview </a>
          <!-- <a id="prices" href='#'> Prices </a> -->
      </nav>
    </div>
    <div v-if="tripHasContent" class='overview-wrapper' id="overview-wrapper">
      <div v-for="ev in events">
        <day v-bind:events=ev> </day>
      </div>
    </div>
    <div v-else class="warning-wrapper" id="no-content">
      Looks like you haven't added anything to the trip 🙈 <br>
      Start by clicking the button below.
    </div>
    <div class="form-wrapper" id="flight-form">
      <addflightform></addflightform>
    </div>
    <div class="form-wrapper" id="visit-form">
      <addvisitform></addvisitform>
    </div>
    <div class="form-wrapper" id="hotel-form">
      <addaccomdationform></addaccomdationform>
    </div>
    <div class="form-wrapper" id="new-trip-form">
      <createnewtripform></createnewtripform>
    </div>
    <div class="fab-wrapper">
      <fab :actions="fabActions"
       @addFlight="addFlight"
       @addHotel="addHotel"
       @addVisit="addVisit"
       @toOverview="toOverview"
       :position-type="positiontype"
       :bg-color="bgColor"
       :icon-size="iconsize"
        ></fab>
    </div>
  </div>

</template>

<script>
import Day from './Day.vue'
import fab from 'vue-fab'
import addflightform from './AddFlightForm.vue'
import addvisitform from './AddVisitForm.vue'
import addaccomdationform from './AddAccomodationForm.vue'
import createnewtripform from './NewTripForm.vue'


var t = JSON.parse(localStorage.getItem("trip"))
var t = JSON.parse(localStorage.getItem("trip"));
var hasContent = true;
var events = []
if (t == null) {
    hasContent = false;
    //TODO: set the old trip data to user input
    localStorage.setItem('trip',JSON.stringify({"name":"Best Holiday","members":"Freddie","plan":{}}))
    t = JSON.parse(localStorage.getItem('trip'));
  } else {
    if (Object.keys(t.plan).length === 0 && t.plan.constructor === Object) {
      hasContent = false;
    } else {
      for (var key in t.plan) {
        events.push(t.plan[key])
      }
      console.log(events.length)
    }
  }

export default {
  components : {
    "day" : Day,
    'fab': fab,
    "addflightform":addflightform,
    "addvisitform":addvisitform,
    "addaccomdationform":addaccomdationform,
    "createnewtripform":createnewtripform
  },
  data() {
    return {
      'tripHasContent' :hasContent,
      'title': t.name,
      'members':t.members,
      'events':events,
      'bgColor': '#DB222A',
      'positiontype': "fixed",
      'iconsize': "small",
            'fabActions': [
                {
                    'name': 'addFlight',
                    'icon': 'flight',
                    'tooltip': 'Add Flight'
                },
                {
                    'name': 'addHotel',
                    'icon': 'hotel',
                    'tooltip': 'Add Accomodation'
                },
                {
                    'name': 'addVisit',
                    'icon': 'account_balance',
                    'tooltip': 'Add Attraction'
                },
                {
                    'name': 'toOverview',
                    'icon': 'home',
                    'tooltip': 'Return to Overview'
                }
            ]
      }
  },
  methods:{
     addFlight(){
       $('#no-content').hide();
       $('#overview-wrapper').hide();
       $('#visit-form').hide();
       $('#hotel-form').hide();
       $('#flight-form').show();
       $("#new-trip-form").hide()
     },
     addVisit(){
       $('#no-content').hide();
       $('#overview-wrapper').hide();
       $('#flight-form').hide();
       $('#visit-form').show();
       $('#hotel-form').hide();
       $("#new-trip-form").hide()


     },
     addHotel(){
       $('#no-content').hide();
       $('#overview-wrapper').hide();
        $('#flight-form').hide();
       $('#visit-form').hide();
       $('#hotel-form').show();
       $("#new-trip-form").hide()

     },
     toOverview() {
       $('#no-content').hide();
       $('#overview-wrapper').show();
        $('#flight-form').hide();
       $('#visit-form').hide();
       $('#hotel-form').hide();
       $("#new-trip-form").hide()

     }
 }
}
</script>

<style>

.overview-header{
  background-color: #BFDBF7;
  width:100%;
  line-height: 0.5em;
  margin-left: -20px;
  padding-left: 20px;
  margin-top: -25px;
  padding-top: 20px;
  padding-bottom: 0.5em;
  box-shadow: 0px 5px 7px -3px rgba(0,0,0,0.42);
}
.overview {
  margin: 0;
  padding-left: 10px;
  display: flex;
  flex-direction: column;
  height: 100%;
}
.overview-title {
  font-size: 1.5em;
  padding-left: 10px
}
.overview p:nth-child(2) {
  font-size: 0.8em;
  line-height: 1em;
  padding-left: 10px
}
.overview p:nth-child(3) {
  font-size: 0.8em;
  line-height: 1em;
  padding-left: 10px
}
.fab-wrapper {
  background-color: none;
  position: absolute;
  padding-bottom: 10px;
  margin-bottom: 30px;
  right: 0px;
  font-family: "Roboto",sans-serif;
}
.overview-wrapper {
  overflow: scroll;
  max-height: 500px;
  padding-left: 10px;
}
.detail-nav {
  width: 100%;
  display: flex;
  background-color: none;
  justify-content: space-around;
  padding: 10px;
  margin-left: -20px;
  margin-top: 10px;
}
.detail-nav * {
  text-decoration: none;
  color: black;
}
.detail-nav *:hover {
  color: white;
}
.warning-wrapper {
  word-wrap: break-word;
  height: 100%;
  margin-left: -10px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  text-align: justify;
}
.overview-members {
  margin-left: 10px;
  line-height: 0.5em;
  font-size: 14px;
  text-align: left;
  flex-grow: 0;
}
.form-wrapper {
  display: none;
  word-wrap: break-word;
  height: 100%;
  margin-left: -10px;
  align-items: center;
  justify-content: center;
  padding: 20px;
  text-align: left;
}
</style>
