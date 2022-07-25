<template>
<div id="barchart-container">
  <h5> Character Occurrences and Sentiments</h5>
  <div id="barchart"></div>
  <BarChartLegend/>
</div>
</template>

<script>
import * as d3 from "d3";
import BarChartLegend from "@/components/BarChartLegend"

import fakeChar from "@/data/barcharts/000_Chapterlegend.json";
import Rodion from "@/data/barcharts/001_Rodion Romanovitch Raskolnikov.json"
import Lizaveta from "@/data/barcharts/002_Lizaveta Ivanovna.json";
import Katerina from "@/data/barcharts/003_Katerina Ivanovna.json";
import Sofya from "@/data/barcharts/004_Sofya Semyonovna Marmeladov.json";
import Sonia from "@/data/barcharts/005_Sonia.json";
import Pyotr from "@/data/barcharts/006_Pyotr Petrovitch Luzhin.json";
import Nastasya from "@/data/barcharts/007_Nastasya.json";
import Dounia from "@/data/barcharts/008_Dounia.json";
import Marfa from "@/data/barcharts/009_Marfa Petrovna.json";
import Avdotya from "@/data/barcharts/010_Avdotya Romanovna.json";
import Razumihin from "@/data/barcharts/011_Razumihin.json";
import Nikolay from "@/data/barcharts/012_Nikolay.json";
import Porfiry from "@/data/barcharts/013_Porfiry Petrovitch.json";
import Andrey from "@/data/barcharts/014_Andrey Semyonovitch Lebeziatnikov.json";
import Amalia from "@/data/barcharts/015_Amalia Ivanovna.json";


export default {
  components: {BarChartLegend},
  mounted() {
    this.init(this.concatData(fakeChar), "Chapter");
    this.init(this.concatData(Rodion), "Rodion Raskolnikov");
    this.init(this.concatData(Lizaveta), "Lizaveta Ivanovna");
    this.init(this.concatData(Katerina), "Katerina Ivanovna");
    this.init(this.concatData(Sofya), "Sofya Semyonovna");
    this.init(this.concatData(Sonia), "Sonia");
    this.init(this.concatData(Porfiry), "Porfiry Petrovitch");
    this.init(this.concatData(Nastasya), "Nastasya");     
    this.init(this.concatData(Dounia), "Dounia");
    this.init(this.concatData(Marfa), "Marfa Petrovna");
    this.init(this.concatData(Avdotya), "Avdotya Romanovna");
    this.init(this.concatData(Razumihin), "Razumihin");
    this.init(this.concatData(Nikolay), "Nikolay");
    this.init(this.concatData(Pyotr), "Pyotr Luzhin");
    this.init(this.concatData(Andrey), "Andrey Lebeziatnikov");
    this.init(this.concatData(Amalia), "Amalia Ivanovna");
  },
  methods: {
    concatData(data){
      let newData = []
      for (const chapter in data){
        newData = newData.concat(data[chapter]);
      }
      return newData;
    },
    init(data, name) {
      // set the dimensions and margins of the graph
      var margin = {top:0 , right: 0, bottom: 0, left: 110},
          width = 600 - margin.left - margin.right,
          height = 20 - margin.top - margin.bottom;
// append the svg object to the body of the page
      var svg = d3.select("#barchart")
          .append("div")
          .append("svg")
          .attr("style", "outline: solid 2px #bdbdbd")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.top + margin.bottom)
          .attr("id", name === "Chapter" ? "fake-char" : "")
          .attr("style", name === "Chapter" ? "outline: none" : "outline: solid 1px #bdbdbd")
          .append("g")
          .attr("transform",
              "translate(" + margin.left + "," + margin.top + ")")
// X axis
      var x = d3.scaleBand()
          .range([20, width])
          .domain(data.map(function (d) {
            return d.Segment;
          }))
          .padding(0.4);
      svg.append("g")
//Getting max value to set length of y-axis
      var maxValue = Math.max(...data.map(o => o.Value))

// Add Y axis
      var y = d3.scaleLinear()
          .domain([0, maxValue])
          .range([height, 0]);
      svg.append("g")
      svg.append("text")
          .style("fill", "#313036")
          .style("font-size", "11px")
          .attr("class", "y label")
          .attr("text-anchor", "end")
          .attr("y", 1)
          .attr("dy", name === "Chapter" ? "1.5em" : "1.1em")
          .attr("dx", "1em")
          .text(name);

// Bars
      svg.selectAll("mybar")
          .data(data)
          .enter()
          .append("rect")
          .attr("class", name === "Chapter" ? "" : "real-bar")
          .attr("x", function (d) {
            return x(d.Segment);
          })
          .attr("y", function (d) {
            return y(d.Value);
          })
          .attr("width", x.bandwidth())
          .attr("height", function (d) {
            return height - y(d.Value);
          })
          .attr("fill", function(d) {
            return d.Color;
          })
      
      if(name === "Chapter") {     
      const legendChapters = ["1", "6", "11", "16", "21", "26", "31", "36", "39"]
      const isFirstSegment = (d) => {
        const segmentsOfChapter = data.filter(item => item.Chapter === d.Chapter);
        const firstSegmentOfChapter = segmentsOfChapter[0] 
        return d.Segment === firstSegmentOfChapter.Segment;
      } 
      const countSegmentsOfChapter = (d) => {
        return data.filter(item => item.Chapter === d.Chapter).length;
      }
      const nrSegmentsPerChapter = []
      for (const chapter in fakeChar){
        nrSegmentsPerChapter.push(fakeChar[chapter].length)
      }

        //Add Chapter legend 
        svg.selectAll("chapter-number")
        .data(data)
        .enter()
        .append("text")
        .attr("class", "chapter-number")
        .text(function(d) {
          return legendChapters.includes(d.Chapter.slice(8)) && isFirstSegment(d) ? d.Chapter.slice(8) : ""; //remove "Chapter ", take only chapter number
        })
          .attr("text-anchor", "start")
          .attr("fill", "black")
          .style("font-size", "12px")
          .attr("x", function(d) {
              return x(d.Segment);
              // return x(d.Segment) + x.bandwidth()*countSegmentsOfChapter(d) + x.bandwidth();
          })
          .attr("y", function(d) {
              return height - y(d.Value) - 2;
          });

      // Add chapter separators
      svg
      .selectAll("chapter-line")
      .data(data)
      .enter()
      .append("line")
        .attr('x1', function(d) {
            return legendChapters.includes(d.Chapter.slice(8)) && isFirstSegment(d) ? x(d.Segment) : 0;} )
        .attr('x2', function(d) {
            return legendChapters.includes(d.Chapter.slice(8)) && isFirstSegment(d) ? x(d.Segment) : 0;} )
        .attr('y1', function(d) {
            return legendChapters.includes(d.Chapter.slice(8)) && isFirstSegment(d) ? x(data[0].Segment) + x.bandwidth()*countSegmentsOfChapter(data[0]) + x.bandwidth() - height/2 : 0;} )
        .attr('y2', function(d) {
            return legendChapters.includes(d.Chapter.slice(8)) && isFirstSegment(d) ? x(data[0].Segment) + x.bandwidth()*countSegmentsOfChapter(data[0]) + x.bandwidth() + (height+margin.top+margin.bottom)*19.05: 0;} )
        .attr('stroke', 'rgba(189, 189, 189, 0.5)')
        .attr('stroke-width', 1.5)
      }
    }

    }

  
};
</script>
<style>
@import url('https://fonts.googleapis.com/css?family=PT+Sans');
body {
  font-family: 'PT Sans', sans-serif;
  background-color: #eee;
}
#barchart {
  margin-left: 20px;
}

.title {
  position: absolute;
  text-align: center;
  left: 2em;
}
h1, a {
  color: #18A999;
  text-decoration: none;
}
ul.menu {
  list-style: none;
  position: absolute;
  z-index: 100;
  min-width: 20em;
  text-align: left;
}
ul.menu li {
  margin-top: 0;
  position: relative;
}

svg {
  overflow: visible;
}
</style>