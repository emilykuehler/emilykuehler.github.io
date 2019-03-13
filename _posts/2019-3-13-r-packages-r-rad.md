# My First Non-Trivial R Package

## Project Goals

My love of data and the NBA naturally pushes me toward a lot of side projects involving basketball. These projects inevitably involve some sort of data retrieval and visualization. After scraping data from multiple sources, generally thinking about gathering in a way specific to whatever project I was doing, I decided to get organized and write a package that can gather the data I need and make some simple visualizations.

As it turns out, there is already a CRAN package, ([Alex Bresler's excellent nbastatR](http://asbcllc.com/nbastatR/)) so you may be asking why I am going through the somewhat masochistic motions of writing my own. 

* I wanted to gain experience creating a non-trivial R package and this seemed like an ideal project.
* I wanted to add visualization features (i.e. the ability to create shot charts along these lines:
<br/>
<img src="/img/klayshotchart.jpeg" alt="klay" width="450"/>
<br/>


## Getting Started

There are already a lot of resources out there to get you started with creating your first R package. The two that I relied on most for setting up this package were:<br/>
* Hilary Parker's classic ['Writing an R package from scratch'](https://hilaryparker.com/2014/04/29/writing-an-r-package-from-scratch/)
* Fong Chun Chan's ['Making Your First R Package'](https://hilaryparker.com/2014/04/29/writing-an-r-package-from-scratch/)