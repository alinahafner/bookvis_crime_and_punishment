<template>
  <div id="network">
    <h5> Character Network</h5>
  </div>
</template>

<script>

import * as d3 from "d3";
import cooccurrences from "@/data/network/cooccurrences.json";
import cloud from "d3-cloud";

export default {
  components: {},
  mounted() {
    this.init();
  },
  data() {
    return {characterKey: ""}
  },
  watch: {
    characterKey: {
      deep: true,
      handler() {
        this.initWordCloud()
      }

    }
  },
  methods: {

    onChange(event, name, color) {
      this.$emit("changeCharacter", {charName: name, charColor: color})
    },

    init() {
      const margin = {top: 2, right: 5, bottom: 10, left: 10},
          width = 600 - margin.left - margin.right,
          height = 500 - margin.top - margin.bottom;

      const svg = d3.select("#network")
          .append("svg")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.top + margin.bottom)
          .append("g")
          .attr("transform",
              `translate(${margin.left}, ${margin.top})`);

      const data = cooccurrences;

      /* eslint-disable */

      //Scales
      let colorNode = d3.scaleOrdinal(d3.schemeTableau10.concat(d3.schemeDark2)); //change color theme from schemeCategory10

      //Getting max value to set domain
      var maxValue = Math.max(...data.nodes.map(o => o.size))

      let nodeSize = d3.scaleLinear()
          .domain([0, maxValue]) // unit: occurences
          .range([5, 15]) // unit: pixels


      let simulation = d3.forceSimulation()
          .force("link", d3.forceLink().id(function (d) {
            return d.id;
          }).distance(10).strength(0.09))
          .force("charge", d3.forceManyBody().strength(-170))
          .force("center", d3.forceCenter(width/ 2, height / 2))

      let strokeWidth = d3.scaleLinear()
          .domain([1, 343]) // unit: occurences
          .range([0.5, 4]) // unit: pixels


      let link = svg.append("g")
          .selectAll("line")
          .data(data.links)
          .enter().append("line")
          .attr("stroke-width", function (d) {
            return strokeWidth(d.strokewidth);
          })
          .attr("stroke", function (d) {
            return d.color;
          })

      let node = svg.append("g")
          .attr("class", "nodes")
          .selectAll("g")
          .data(data.nodes)
          .enter().append("g")
          .on("click", (event, d) => this.onChange(event, d.name, d.color))

      let circles = node.append("circle")
          .attr("r", d => nodeSize(d.size))
          .attr("fill", function(d) {
            return d.color;
          })

      let lables = node.append("text")
          .text(function (d) {
            return d.name;
          })
          .attr('x', -4)
          .attr('y', d => d.name == "Rodion Romanovitch Raskolnikov" || d.name == "Sonia" ? -20 : d.name == "Katerina Ivanovna" ? -25 : -15)
          .attr("fill", function(d) {
            return d.color;
          })
          .style("font-size", "11px")
          .on("click", (event, d) => this.onChange(event, d.name, d.color))

      node.append("title")
          .text(function (d) {
            return d.name + ' occurred ' +d.size + " times in the book"
          });


      // Create a drag handler and append it to the node object instead
      let drag_handler = d3.drag()
          .on("start", dragstarted)
          .on("drag", dragged)
          .on("end", dragended);

      drag_handler(node);

      simulation
          .nodes(data.nodes)
          .on("tick", ticked);

      simulation.force("link")
          .links(data.links);

      function ticked() {
        link
            .attr("x1", function (d) {
              return d.source.x;
            })
            .attr("y1", function (d) {
              return d.source.y;
            })
            .attr("x2", function (d) {
              return d.target.x;
            })
            .attr("y2", function (d) {
              return d.target.y;
            });

        node
            .attr("transform", function (d) {
              return "translate(" + d.x + "," + d.y + ")";
            })
      }

      function dragstarted(event, d) {
        if (!event.active) simulation.alphaTarget(0.3).restart();
        d.fx = d.x;
        d.fy = d.y;
      }

      function dragged(event, d) {
        d.fx = event.x;
        d.fy = event.y;
      }

      function dragended(event, d) {
        if (!event.active) simulation.alphaTarget(0);
        d.fx = null;
        d.fy = null;
      }
    },
    initWordCloud(key){

      var myWords = wordsData[key];
      // console.log(data[key]);
// set the dimensions and margins of the graph
      var margin = {top: 2, right: 2, bottom: 2, left: 2},
          width = 450 - margin.left - margin.right,
          height = 450 - margin.top - margin.bottom;


// append the svg object to the body of the page
      var svg = d3.select("#wordcloudcharacter").append("svg")
          .attr("width", width + margin.left + margin.right)
          .attr("height", height + margin.top + margin.bottom)
          .append("g")
          .attr("transform",
              "translate(" + margin.left + "," + margin.top + ")");

// Constructs a new cloud layout instance. It run an algorithm to find the position of words that suits your requirements
// Wordcloud features that are different from one word to the other must be here
      var layout = cloud()
          .size([width, height])
          .words(myWords.map(function (d) {
            return {text: d[0], size: d[1]};
          }))
          .padding(4, 5)        //space between words
          .rotate(function () {
            return ~~(Math.random() * 2) * 90;
          }) // font size of words
          .fontSize(function (d) {
            return d.size;
          })
          .on("end", draw);
      layout.start();

      function getRandomColor() {
        var letters = '0123456789ABCDEF';
        var color = '#';
        for (var i = 0; i < 6; i++) {
          color += letters[Math.floor(Math.random() * 16)];
        }
        return color;
      }

// This function takes the output of 'layout' above and draw the words
// Wordcloud features that are THE SAME from one word to the other can be here
      function draw(words) {
        svg
            .append("g")
            .attr("transform", "translate(" + layout.size()[0] / 2 + "," + layout.size()[1] / 2 + ")")
            .selectAll("text")
            .data(words)
            .enter().append("text")
            .style("font-size", function (d) {
              return d.size;
            })
            .attr("fill", function () {
              return getRandomColor();
            })
            .attr("text-anchor", "middle")
            .style("font-family", "Impact")
            .attr("transform", function (d) {
              return "translate(" + [d.x, d.y] + ")rotate(" + d.rotate + ")";
            })
            .text(function (d) {
              return d.text;
            });
      }
    }
    /* eslint-enable */
  }
};
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=PT+Sans');

#network {
  font-family: 'PT Sans', sans-serif;
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
  margin-top: 1em;
  position: relative;
}

.links line {
  stroke-opacity: 0.5;
}

.nodes circle {
  stroke: #fff;
  stroke-width: 1.5px;
  cursor: pointer;
}

text {
  font-family: sans-serif;
  font-weight: bold;
  cursor: pointer;
}
</style>