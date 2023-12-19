<script>
  export default {
    data: () => ({
          message: "My message.",
          counterTitle: "Counter",
          count: 0,
          incrementAmount: 1,
          favoriteList: [],
          dolphins: [
            { tag: 1, name: "Ian Torphe", stroke: ["Freestyle"] },
            { tag: 2, name: "Grant Hackett", stroke: ["Freestyle"] },
            { tag: 3, name: "Susie O'Neil", stroke: ["Butterfly"] },
            { tag: 4, name: "Leisel Jones", stroke: ["Breaststroke"] },
            { tag: 5, name: "Stephanie Rice", stroke: ["Butterfly", "Backstroke", "Breaststroke", "Freestyle"]}
          ],
          newDolphin: { tag: null, name: "", stroke: [] }
        }),
        computed: {
          displayTitle() {
            if (this.count > 10) {
              return "Title Counter > 10"
            } else {
              return "Title Counter"
            }
          },
          strokeStatistics() {
            const strokes = ["Butterfly", "Backstroke", "Breaststroke", "Freestyle"]
            const statistics = { Butterfly: 0, Backstroke: 0, Breaststroke: 0, Freestyle: 0 }
            this.dolphins.forEach(dolphin => {
              strokes.forEach(stroke => {
                if (dolphin.stroke.indexOf(stroke) > -1) {
                  statistics[stroke] += 1
                }
              })
            })
            return statistics
          }
        },
        methods: {
          addNewDoplhin() {
            this.dolphins.push(this.newDolphin)
            this.newDolphin = { tag: null, name: "", stroke: [] }
          },
          favoriteDolphin(dolphin) {
            this.favoriteList.push(dolphin)
          },
          changeIncrementAmount(e) {
            this.incrementAmount = event.target.value;
          },
          incrementCounter() {
            this.count += this.incrementAmount;
          }
        },
        watch: {
          count(newNumber) {
            if (newNumber > 10) {
              this.counterTitle += "Test"
            }
          }
        }
  }
</script>

<template>
  <h1>Hello Vue!</h1>
  <p v-if="message.length % 2 === 0">Even: {{ message }}</p>
  <p v-else>Odd: {{ message }}</p>
  <hr/>
  <h2>{{ displayTitle }}</h2>
  <p> {{ count }} </p>
  <div>
    <label for="incrementAmount">Increment by:</label>
    <input type="number" v-model="incrementAmount" />
  </div>
  <button @click="incrementCounter">Counter</button>
  <hr/>
  <h2>Dolphins</h2>
  <ul>
    <li v-for="(dolphin, index) in dolphins" :key="`dolphin-${index}`">
      <p>#{{ dolphin.tag }} {{dolphin.name}} (<span v-for="(stroke, index) in dolphin.stroke" :key="`stroke-${index}`">{{ stroke }}{{ index === dolphin.stroke.length - 1 ? "" : ", " }}</span>)</p>
      <button @click="favoriteDolphin(dolphin)">Favorite Dolphin</button>
    </li>
  </ul>
  <p><span v-for="(dolphin, index) in listOfDolphins" :key="`dolphin-${index}`">{{ dolphin }}{{ index === listOfDolphins.length - 1 ? "" : ", " }} </span></p>
  <h3>Statistics</h3>
  <ul>
    <li v-for="(stat, stroke) in strokeStatistics" :key="`stroke-${stroke}`">{{stroke}}: {{stat}}</li>
  </ul>
  <h3>Favorite Dolphins</h3>
  <ul v-if="favoriteList.length > 0">
    <li v-for="(dolphin, index) in favoriteList" :key="`dolphin-${index}`">#{{ dolphin.tag }} {{dolphin.name}} (<span v-for="(stroke, index) in dolphin.stroke" :key="`stroke-${index}`">{{ stroke }}{{ index === dolphin.stroke.length - 1 ? "" : ", " }}</span>)</li>
  </ul>
  <p v-else>No favorite dolphins yet.</p>
  <h3>New Dolphin</h3>
  <p>{{ newDolphin }}</p>
  <label for="dolphin-name">Name </label>
  <input type="text" v-model="newDolphin.name" @keyup.enter="addNewDoplhin"/>
</template>
