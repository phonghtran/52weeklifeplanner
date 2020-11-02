<template>
  <div class="container">
    <div class="row">
      <div class="col-12 "><h1>Your Weekly Life Planner</h1></div>
      <div class="col-10">
        <h3>What You Have Control Over</h3>

        <p>{{ timeSpans.freeTime / 52 }} years</p>

        <template v-for="(item, index) in timeSpans.freeTime"
          ><LifeBox v-bind:key="'freeTime' + index" />
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
          <ul>
            <li>timeSpans {{ timeSpans }}</li>

            <li>{{ timeLeft }} timeLeft</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import LifeBox from "@/components/LifeBox.vue";

  export default {
    name: "LifePlanner",
    components: {
      LifeBox,
    },
    computed: {
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

        console.log(this.timeLeft, lifeAtWork, lifeSleeping, lifeEating);

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
        lifeExpectancy: 72,
        retirementAge: 65,
        workWeeklyHours: 40,
      };
    },
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss"></style>
