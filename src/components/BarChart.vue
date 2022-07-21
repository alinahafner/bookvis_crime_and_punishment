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

import Amalia from "@/data/barcharts/Amalia Ivanovna.json";
import Dmitri from "@/data/barcharts/Dmitri Prokofitch.json";
import Ilya from "@/data/barcharts/Ilya Petrovitch.json";
import Katerina from "@/data/barcharts/Katerina Ivanovna.json";
import Lida from "@/data/barcharts/Lida.json";
import Lizaveta from "@/data/barcharts/Lizaveta Ivanovna.json";
import Nastasya from "@/data/barcharts/Nastasya.json";
import Nikodim from "@/data/barcharts/Nikodim Fomitch.json";
import Nikolay from "@/data/barcharts/Nikolay.json";
import Porfiry from "@/data/barcharts/Porfiry Petrovitch.json";
import Pyotr from "@/data/barcharts/Pyotr Petrovitch Luzhin.json";
import Razumihin from "@/data/barcharts/Razumihin.json";
import Rodion from "@/data/barcharts/Rodion Romanovitch Raskolnikov.json";
import Sofya from "@/data/barcharts/Sofya Semyonovna.json";
import Sonia from "@/data/barcharts/Sonia.json";


export default {
  components: {BarChartLegend},
  mounted() {
    this.init(Amalia, "Amalia Ivanovna");
    this.init(Dmitri, "Dmitri Prokofitch");
    this.init(Ilya, "Ilya Petrovitch");
    this.init(Katerina, "Katerina Ivanovna");
    this.init(Lida, "Lida");
    this.init(Lizaveta, "Lizaveta Ivanovna");
    this.init(Nastasya, "Nastasya");
    this.init(Nikodim, "Nikodim Fomitch");
    this.init(Nikolay, "Nikolay");
    this.init(Porfiry, "Porfiry Petrovitch");
    this.init(Pyotr, "Pyotr Petrovitch Luzhin");
    this.init(Razumihin, "Razumihin");
    this.init(Rodion, "Rodion Romanovitch Raskolnikov");
    this.init(Sofya, "Sofya Semyonovna");
    this.init(Sonia, "Sonia");
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
      var margin = {top:0 , right: 0, bottom: 0, left: 100},
          width = 500 - margin.left - margin.right,
          height = 20 - margin.top - margin.bottom;
// append the svg object to the body of the page
      var svg = d3.select("#barchart")
          .append("div")
          .append("svg")
          .attr("style", "outline: solid 2px #bdbdbd")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.top + margin.bottom)
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
          .attr("dy", "0.9em")
          .attr("dx", "1em")
          .text(name);

// Bars
      svg.selectAll("mybar")
          .data(data)
          .enter()
          .append("rect")
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

      //Add Chapter legend
      // const legendChapters = ["1", "6", "11", "16", "21", "26", "31", "36", "38"]
      // const isFirstSegment = (d) => {
      //   const segmentsOfChapter = data.filter(item => item.Chapter === d.Chapter);
      //   const firstSegmentOfChapter = segmentsOfChapter[0] 
      //   return d.Segment === firstSegmentOfChapter.Segment;
      // } 
      // const countSegmentsOfChapter = (d) => {
      //   return data.filter(item => item.Chapter === d.Chapter).length;
      // }
      // const nrSegmentsPerChapter = []
      // for (const chapter in fakeChar){
      //   nrSegmentsPerChapter.push(fakeChar[chapter].length)
      // }
      
      // if(name === "Chapter") {
      //   svg.selectAll("chapter-number")
      //   .data(data)
      //   .enter()
      //   .append("text")
      //   .attr("class", "chapter-number")
      //   .text(function(d) {
      //     return legendChapters.includes(d.Chapter.slice(8)) && isFirstSegment(d) ? d.Chapter.slice(8) : ""; //remove "Chapter ", take only chapter number
      //   })
      //     .attr("text-anchor", "end")
      //     .attr("fill", "black")
      //     .style("font-size", "12px")
      //     .attr("x", function(d) {
      //         return x(d.Segment) + x.bandwidth()*countSegmentsOfChapter(d) + x.bandwidth();
      //     })
      //     .attr("y", function(d) {
      //         return height - y(d.Value) - 2;
      //     });
      // }
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



</style>