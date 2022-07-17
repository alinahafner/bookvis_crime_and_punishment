<template>
  <div id="app">
    <div id="header">
      <div id="div1">
        <H1>BookVisualizer - Crime and Punishment</H1>
      </div>
    </div>
    <h3>Overview on General Information & Book Statistics</h3>
    <div id="main1"><img :src="cover" :width="200" :height="200"/>
      <div id="textinfo">
        <div id="list1">
          <ul>
            <li>Title: Crime and Punishment</li>
            <li>Author: Fjodor Michailowitsch Dostojewski</li>
            <li>Published in 1866</li>
            <li>Suitable for 8th and 9th grade</li>
            <li>Estimated reading time: 15 hours 30 minutes</li>
          </ul>
        </div>
        <div id="list2">
          <ul>
            <li>Pages: 643</li>
            <li>Total number of words: 186703</li>
            <li>Chapter length (avg.): 16 pages</li>
            <li>Sentence length (avg.): 17 words</li>
            <li>Main settings: Dounia, Pole and Petersburg</li>
          </ul>
        </div>
      </div>
      <div id="difflevel"><h4>Difficulty Level</h4>
        <ReadabilityPieChart></ReadabilityPieChart>
      </div>
      <div id="conversational"><h4>Conversational Activity</h4>
        <QuotationPieChart></QuotationPieChart>
      </div>
    </div>
    <h3>Overview on Characters</h3>
    <div id="main2">
      <BarChart></BarChart>
      <NetworkDiagram v-on:changeCharacter="characterChange($event)"></NetworkDiagram>
    </div>
  </div>
</template>

<script>
import NetworkDiagram from "@/components/NetworkDiagram";
import BarChart from "@/components/BarChart";
import ReadabilityPieChart from "@/components/ReadabilityPieChart";
import QuotationPieChart from "@/components/QuotationPieChart";

export default {
  name: 'App',
  data() {
    return {
      cover: require('./assets/cover.svg'),
      showBubbleChart: true,
      chapterKey: "",
      characterKey: "",
      topicKey: "",
    };
  },
  components: {
    BarChart,
    NetworkDiagram,
    ReadabilityPieChart,
    QuotationPieChart

  },
  methods: {
    toggle(e) {
      if (!e.target.classList.contains('btn-active')) {
        this.showBubbleChart = !this.showBubbleChart;
      }
    },
    chapterChange(event) {
      this.chapterKey = event;
    },
    characterChange(event) {
      this.characterKey = event;
    },
    getTopic(event) {
      this.topicKey = event;
    }
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=PT+Sans');

body {
  font-family: 'PT Sans', sans-serif;
  background-color: #eee;
}

#app {
  background: #F6F9F1;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #A72608;
}


#div1 {
  background: #A72608;
  padding: 5px;
  width: auto;
  height: 6px;
  display: inline-block;
  flex-direction: column;
  justify-content: left;
  text-align: center;
  line-height: 4px;
  margin-bottom: 10px;
}

#header {
  padding: 10px;
  background: #A72608;
  margin-bottom: 10px;
}

h1 {
  color: #E6EED6;
  text-align: center;
  font-size: 28px;
}

h3 {
  color: #2E4052;
  text-align: left;
  font-size: 20px;
  margin-left: 20px;
  margin-bottom: 0px;
}


#main1 {
  display: grid;
  row-gap: 0px;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 0px;
  grid-auto-rows: minmax(100px, auto);
  grid-column: 3/3;
  grid-row: 1;
  margin-bottom: 10px;
  margin-top: 10px;
  border-bottom: 2px solid #bdbdbd;
  padding: 5px;
}

#main2 {
  display: grid;
  row-gap: 10px;
  row-gap: 10px;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 10px;
  grid-auto-rows: minmax(100px, auto);
  grid-column: 3/3;
  grid-row: 1;
  margin-bottom: 10px;
  border-bottom: 2px solid #bdbdbd;
  padding: 10px;
}

#main3 {
  display: grid;
  row-gap: 5px;
  row-gap: 5px;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 5px;
  grid-auto-rows: minmax(100px, auto);
  grid-column: 2/2;
  grid-row: 1;
  margin-bottom: 10px;
  margin-top: 10px;
}


#submain3 {
  margin-bottom: 5px;
}

#submain3-buttons button {
  border: 1px solid #bbc1be;
  font-size: 16px;
  font-weight: bold;
  padding: 10px 24px;
  cursor: pointer;
  transition: 0.3s;
  box-shadow: 5px 5px 10px rgba(0, 0, 0, 0.05);
}

#submain3-buttons button:first-child {
  border-radius: 5px 0 0 5px;
}

#submain3-buttons button:last-child {
  border-radius: 0 5px 5px 0;
}

#submain3-buttons button:not(:last-child) {
  border-right: none;
}

#submain3-buttons button:hover {
  background-color: #18A999; /* #3e8e41; */
  color: white;
}

#bubble-chart-container {
  padding: 5px 0;
}

.btn-active {
  background-color: #18A999; /* #3e8e41; */
  color: white;
}

.btn-inactive {
  background-color: white;
  color: #18A999; /* #3e8e41; */
}

.chart-active {
  display: block;
}

.chart-inactive {
  display: none;
}


.arrow {
  width: 120px;
  position: relative;
}

.line {
  margin-top: 14px;
  width: 90px;
  background: blue;
  height: 10px;
  float: left;
}

.point {
  width: 0;
  height: 0;
  border-top: 20px solid transparent;
  border-bottom: 20px solid transparent;
  border-left: 30px solid blue;
  float: right;
}

ul {
  list-style-type: "📗";
  margin-top: 0px;
  margin-left: 0px;
  margin: 0px;
}

li {
  text-align: left;
  padding-left: 4px;
  font-size: 14px;
}

#textinfo {
  display: grid;
  row-gap: 1px;
  grid-template-columns: repeat(2, 1fr);
  grid-gap: 0px;
  grid-auto-rows: minmax(100px, auto);
  grid-column: 2/2;
  grid-column-gap: 0px;
  grid-row: 1;
  margin-bottom: 0px;
  margin-top: 0px;
  width: 700px;
  margin-left: 0px;
  margin-right: 0px;
  padding-left: 0px;

}

#list1 {
  margin-left: 0px;
  margin-right: 0px;
  width: 400px;
}

#list2 {
  margin-left: 0px;
  margin-right: 0px;
  width: 400px;
}

#difflevel {
  margin-top: 0px;
}

#conversational {
  margin-top: 0px;
}


</style>