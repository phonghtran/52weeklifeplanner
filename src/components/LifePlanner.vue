<template>
  <div class="container">
    <div class="row">
      <div class="col-12 "><h1>Your Weekly Life Planner</h1></div>
      <div class="col-10">
        <h3>What You Have Control Over</h3>

        <p>{{ Math.floor(timeSpans.freeTime / 52) }} years of free time</p>

        <template v-for="category in hobbiesInWeekUnits">
          <template v-for="(lifeBox, lifeBoxIndex) in category.count">
            <LifeBox
              v-bind:key="'freeTime' + category.class + lifeBoxIndex"
              v-bind:class="freeTimeClass(category.class)"
            />
          </template>
        </template>

        <h3>Your Job Until Retirement</h3>

        <p>
          {{ Math.floor(timeSpans.working * 7) }} days of doing what you love?
        </p>

        <template v-for="(item, index) in timeSpans.working"
          ><LifeBox
            v-bind:key="'working' + index"
            v-bind:class="{ '--working': true }"
          />
        </template>
        <h3>Eating Time</h3>

        <p>
          {{ Math.floor(timeLeft * 21) }} meals yet to be eaten. How many will
          you enjoy? How many with people you love?
        </p>

        <template v-for="(item, index) in timeSpans.eating"
          ><LifeBox
            v-bind:key="'eating' + index"
            v-bind:class="{ '--eating': true }"
          />
        </template>

        <h3>Weeks Spent Sleeping</h3>

        <p>
          {{ Math.floor(timeSpans.sleeping * 168) }} hours of restful dreams?
        </p>

        <template v-for="(item, index) in timeSpans.sleeping"
          ><LifeBox
            v-bind:key="'sleeping' + index"
            v-bind:class="{ '--sleeping': true }"
          />
        </template>

        <h3>Life Already Lived</h3>

        <p>{{ timeSpans.expired / 52 }} years of no regrets?</p>

        <template v-for="(item, index) in timeSpans.expired"
          ><LifeBox
            v-bind:key="'expired' + index"
            v-bind:class="{ '--expired': true }"
          />
        </template>
      </div>
      <div class="col-2 sticky">
        <div>
          <label for="lifeExpectancy">Expected Lifespan</label> <br />
          <input id="lifeExpectancy" type="text" v-model="lifeExpectancy" />
        </div>
        <div>
          <label for="currentAge">Current Age</label> <br />
          <input id="currentAge" type="text" v-model="currentAge" />
        </div>
        <div>
          <label for="averageSleepHours">Average Sleep Hours</label> <br />
          <input
            id="averageSleepHours"
            type="text"
            v-model="averageSleepHours"
          />
        </div>
        <div>
          <label for="retirementAge">Retirement Age</label> <br />
          <input id="retirementAge" type="text" v-model="retirementAge" />
        </div>
        <div>
          <label for="workWeeklyHours">Current Job Weekly Hours</label> <br />
          <input id="workWeeklyHours" type="text" v-model="workWeeklyHours" />
        </div>

        <div>
          <h3>Activities</h3>
          <button @click="activityAdd()">
            Add Activity
          </button>
          <Activity
            v-bind:count="activity.count"
            v-bind:key="index"
            v-bind:name="activity.name"
            v-bind:offset="index"
            v-for="(activity, index) in computedHobbies"
            v-on:activityDelete="activityDelete"
            v-on:activityNewName="activityNewName"
          />
        </div>

        <div>
          <ul>
            <li>timeSpans {{ timeSpans }}</li>

            <li>{{ timeLeft }} timeLeft</li>
            <li>{{ hobbies }} hobbies</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Activity from "@/components/Activity.vue";
  import LifeBox from "@/components/LifeBox.vue";

  export default {
    name: "LifePlanner",
    components: { Activity, LifeBox },
    computed: {
      computedHobbies: function() {
        return this.hobbies;
      },
      hobbiesInWeekUnits: function() {
        // if (this.hobbies.length === 0) {
        return [
          {
            class: "",
            count: this.timeSpans.freeTime,
          },
          {
            class: "--sleeping",
            count: this.timeSpans.freeTime,
          },
        ];
        // }
      },
      timeLeft: function() {
        return (this.lifeExpectancy - this.currentAge) * 52;
      },
      timeSpans: function() {
        const lifeAtWork = Math.floor(
          (this.workWeeklyHours / 168) *
            ((this.retirementAge - this.currentAge) * 52)
        );

        const lifeSleeping = Math.floor(
          ((this.averageSleepHours * 7) / 168) * this.timeLeft
        );

        // 2.5 hours eating daily
        const lifeEating = Math.floor(0.1041666667 * this.timeLeft);

        return {
          eating: lifeEating,
          expired: this.currentAge * 52,
          freeTime: this.timeLeft - lifeAtWork - lifeSleeping - lifeEating,
          sleeping: lifeSleeping,
          working: lifeAtWork,
        };
      },
    },
    data: function() {
      return {
        averageSleepHours: 8,
        currentAge: 34,
        hobbies: [{ name: "Activity Name", count: 0 }],
        lifeExpectancy: 72,
        retirementAge: 65,
        workWeeklyHours: 40,
      };
    },
    methods: {
      activityAdd: function() {
        this.hobbies.push({ name: "Activity Name", count: 0 });
      },
      activityDelete: function(offset) {
        if (this.hobbies.length > 1) {
          let newArray = [...this.hobbies];

          newArray.splice(offset, 1);

          this.hobbies = [...newArray];
        }
      },
      activityNewName: function(activityProp) {
        console.log(activityProp);
        this.hobbies[activityProp[1]]["name"] = activityProp[0];
      },
      freeTimeClass: function(category) {
        let classObject = {};
        classObject[category] = true;

        return classObject;
      },
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
