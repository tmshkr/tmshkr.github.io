---
layout: post
title: "Data Visualization"
subtitle: "with D3.js"
author: "Tim"
---

I've recently learned some valuable skills, building upon previous knowledge in
web development, by completing the Data Visualization
Certification program offered by [freeCodeCamp](https://learn.freecodecamp.org/).

<a href="https://www.freecodecamp.org/certification/tmshkr/data-visualization" target="_blank">
<img src="../assets/blog/images/fcc-dataviz-cert.png" />
</a>

Data visualization is simply the graphical representation of information, e.g.,
charts, graphs, tables, and so forth, which makes it easier for humans to interpret
and reason about data in order to make informed decisions. For example, one might
reasonably assume that the upward trend in GDP, which has lasted for over 65 years
as demonstrated in the chart below, will likely continue to persist in the future:

<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/5.7.0/d3.min.js"></script>
<style>
#gdp {
  text-align: center;
  font-family: sans-serif;
  color: #fff;
}

#gdp svg {
  transform: translateX(-40px);
}

rect.bar {
  fill: #007f80;
}

#tooltip {
  position: absolute;
  font-family: sans-serif;
  text-align: center;
  padding: 0.5em;
  white-space: nowrap;
  background-color: #00f;
  visibility: hidden;
  opacity: 0;
}

#tooltip text {
  display: block;
}
</style>
<div id="gdp">
  <h1 id="title">United States GDP</h1>
</div>
<script>
// Acknowledgement:
// @Christian-Paul
// https://codepen.io/freeCodeCamp/pen/GrZVaM

const width = 800;
const height = 500;
var svg = d3.select("#gdp")
            .append("svg")
            .attr("width", width + 60)
            .attr("height", height + 20);

var tooltip = d3.select("body")
                .append("div")
                .attr("id", "tooltip");

var tooltipGDP = tooltip.append("text")
                        .text("GDP")

var tooltipDate = tooltip.append("text")
                         .text("Date")

function formatDate(date) {
    let quarter;
    let month = date.substring(5,7);
    
    if(month === '01') {
      quarter = 'Q1';
    }
    else if (month === '04'){
      quarter = 'Q2';
    }
    else if(month === '07') {
      quarter = 'Q3';
    }
    else if(month ==='10') {
      quarter = 'Q4';
    }

    return `${date.substring(0, 4)} ${quarter}`;
  }

  function formatGDP(GDP) {
    return `$${GDP}B`
  }


d3.json('https://raw.githubusercontent.com/FreeCodeCamp/ProjectReferenceData/master/GDP-data.json').then(json => {
  
  var years = json.data.map( d => d[0].substring(0, 4));
  var GDP = json.data.map(d => d[1]);
  var gdpMin = d3.min(GDP);
  var gdpMax = d3.max(GDP);
  
  var xScale = d3.scaleLinear()
                 .domain([d3.min(years), d3.max(years)])
                 .range([0, width]);
    
  var yScale = d3.scaleLinear()
                 .domain([gdpMin, gdpMax])
                 .range([height, 0]);
  
  var xAxis = d3.axisBottom(xScale)
                .tickFormat(d3.format("d"));
  
  var dx = width / years.length;
  
  var yAxis = d3.axisLeft(yScale);
  
  var scaleGDP = d3.scaleLinear()
                   .domain([gdpMin, gdpMax])
                   .range([(gdpMin/gdpMax) * height, height]);
  
  var scaledGDP = GDP.map(item => scaleGDP(item));
    
  svg.append("g")
     .attr("transform", `translate(40, ${height + 3})`)
     .attr("id", "x-axis")
     .call(xAxis);
    
  svg.append("g")
     .attr("transform", "translate(40, 3)") // +3 to prevent cutting off top of tick label
     .attr("id", "y-axis")
     .call(yAxis);  
  

  svg.selectAll("rect")
     .data(scaledGDP)
     .enter()
     .append("rect")
     .attr('class', 'bar')
     .attr('data-date', (d, i) => json.data[i][0])
     .attr('data-gdp', (d, i) => json.data[i][1])
     .attr("x", (d, i) => 41 + i * dx)
     .attr("y", d => height + 3 - d)
     .attr("width", dx * .9)
     .attr("height", d => d)
     .on('mouseover', function(){
        let x = `${d3.event.pageX + 15}px`;
        let y = `${d3.event.pageY + 15}px`;
        this.style.fill = "#0ff";
        tooltip.style("left", x)     
               .style("top", y)
               .style("right", "")     
               .style("bottom", "")
               .attr('data-date', this.dataset.date)
               .style("visibility", "visible")
               .transition().duration(0)
               .style("opacity", 1);
    
        let bound = document.getElementById("tooltip").getBoundingClientRect();
        let viewport = {
          width: document.documentElement.clientWidth,
          height: document.documentElement.clientHeight
          }
        if (bound.right > viewport.width) {
          x = `${viewport.width - d3.event.pageX + 15}px`;
          tooltip.style("left", "")     
                 .style("right", x)     
        }
        if (bound.bottom > viewport.height) {
          y = `${viewport.height - d3.event.pageY + 15}px`;
          tooltip.style("top", "")
                 .style("bottom", y)
        }
        
        tooltipGDP.text(formatGDP(this.dataset.gdp));
        tooltipDate.text(formatDate(this.dataset.date));               
      })
     .on("mouseout", function() {
        this.style.fill = null;
        tooltip.transition().duration(100)
               .style("opacity", 0)
               .transition().duration(100)
               .style("visibility", "hidden");
      });

});
</script>


Based on such a reasonable assumption, that the monetary value of goods and services
produced by the United States will continue to grow, rational agents can make informed financial
decisions, e.g., to hire employees, purchase a home, and so forth. Of course this
could also be accomplished with raw data, but when
data is presented in a visually engaging format, certain trends and patterns become
much more apparent (and much more enjoyable to work with).

The projects I made with freeCodeCamp are available to view and fork on my [CodePen](https://codepen.io/tmshkr/) profile.
The certification program involved several different kinds of data visualization
projects: a [bar chart](https://codepen.io/tmshkr/pen/aRmPxz) (displayed above),
a [heat map](https://codepen.io/tmshkr/pen/bmqKNW), a [scatter plot](https://codepen.io/tmshkr/pen/qJrZJZ),
a [choropleth map](https://codepen.io/tmshkr/pen/EdbQBQ), and a [tree map](https://codepen.io/tmshkr/pen/zmWYMp).
While I'm by no means a D3 master at this point, the program did get me thoroughly
acquainted with the D3 API, so that I have a solid understanding of the basics:
setting up axes, scales, working with datasets, and rendering data with the SVG
coordinate system. I am most grateful to freeCodeCamp for making these exercises
available, so that I could become competent with this popular and
useful JavaScript library in a relatively short period of time.

Knowledge is useless if it can't be accessed, and through techniques like data
visualization, information can be made much more accessible and digestible,
so that it can become more widely useful. This is a valuable skill that I am
excited to be learning.