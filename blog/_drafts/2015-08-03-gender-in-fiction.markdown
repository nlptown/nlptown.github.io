---
layout: post
title:  "Gender Stereotypes in Amateur Fiction"
date:   2015-08-03 9:00:00
tags: NLP gender literature fiction stereotypes
comments: true
---
<script src="http://d3js.org/d3.v3.js"></script>

<p>About a year ago I collected a corpus of English fiction novels from the website Smashwords. It contains
    11,162 books written by amateurs and made available for free on the website.
    Finally I've taken the time to do some analyses and get more insight into how amateur authors write. One of the
    questions I wanted to answer is: what are the differences in characterization between male and female characters?
    Do men and women act and behave the same in these books, do we find the typical stereotypes, or are there any
    unexpected differences?
</p>

<style>
    .graph { color: #444; background: #f3f3f3; font: normal 12px "Adobe Garamond Pro", "Palatino", serif; margin: 2em; }
    header { margin: 20px 0 20px 120px; border-bottom: 1px solid #6c6c6c; width: 360px; position: relative; }
    h2 { font-size: 16px; font-weight: normal; text-shadow: #fff 0 1px 0; margin: 0 0 0 0; padding: 0; }
    small { color: #a3a3a3; font-size: 10px; position: absolute; top: 0.6em; right: 0;}
    a { color: #a3a3a3; }

    text.label { fill: #444; }
    text.label.start { text-anchor: end; }
    line.slope { stroke: #444; stroke-width: 1; }

    .missing text.label { display: none; }
    .missing line.slope { display: none; }

    .over text.label { fill: #bb2629; font-size: 14px !important; font-weight: bold; }
    .over line.slope { stroke: #bb2629; stroke-width: 2; }

    .boxed { border: 1px solid black ; background-color: lightgray; }
    .in_box {margin: 10px 10px 10px 10px; }
</style>

<p>
    To answer these questions, I focused on the use of the pronouns <em>he</em>
    and <em>she</em>. In particular, I analyzed the words that these
    pronouns co-occur with. The idea was that the nouns, adjectives and verbs that appear
    much more often together with <em>he</em> than with <em>she</em> give an accurate picture
    of prototypically male characterization, while those that occur much more frequently together
    with <em>she</em> paint a picture of prototypically female characterization.
</p>

<div class="boxed">
<h4 class="in_box">Technical details</h4>
<p class="in_box">
    I used the
    Stanford Dependency Parser to analyze all sentences from the novels that contained either the pronoun
    <em>he</em> or <em>she</em> &mdash; 5,608,308 sentences in total. I then identified all subject (nsubj)
    relations that these pronouns entered into &mdash; with a noun (<em>s/he is ... &lt;noun&gt;</em>, as in
    <em>he is the <u>king</u></em>), an adjective (<em>s/he is &lt;adjective&gt;</em>, as in <em>she is <u>pregnant</u></em>),
    or a verb (<em>s/he &lt;verb&gt;</em>, as in <em>she <u>giggled</u></em>). I lemmatized these words with
    Stanford CoreNLP and counted how often they occurred with <em>he</em> and <em>she</em>. Below these frequencies
    are given per million relations, so when <em>boy</em> has a frequency of 299.3 for <em>he</em>, this
    means that for every 1,000,000 subject relations that <em>he</em> enters into, 299.3 of these are with
    <em>boy</em>, on average. Finally, I used a chi-square test to determine if the nouns, adjectives or verbs
    occurred more significantly with <em>he</em> than with <em>she</em>, or vice versa, on the basis of their
    absolute frequencies.
</p>
</div><br/>

<p class="indent">
    The nouns give the most predictable results. Here I focus on constructions like <em>he is a hero</em> or
    <em>she is his wife</em>. For every noun in our vocabulary, I count how many times it occurs in this
    type of construction. When I find it at least 100 times, I determine whether it occurs significantly
    more frequently with <em>he</em> or with <em>she</em> (on the basis of a chi square test). The slopegraph
    below shows the top 10 differences for either case. To the left, it lists the number of times each noun
    co-occurs with <em>he</em>, and to the right, it has the number of times each noun co-occurs with <em>she</em>
    (per million constructions). Most of the results are unsurprising: in the books I analyzed,
    men are <em>boys</em>, <em>sons</em>,
    <em>brothers</em> or <em>kings</em>, while women tend to be <em>girls</em>, <em>daughters</em> or
    <em>princesses</em>. Still, the top tens contain a few surprises: <em>leader</em> and <em>hero</em> are
    used to describe men much more frequently than women; <em>beauty</em> shows the opposite pattern. Coincidence,
    or a first sign that women are described by their looks, and men by their actions? We’ll find out soon.
 </p>

<div class="graph">
    <header><br/>
        <h2>
            s/he is ... &lt;noun&gt;
        </h2>
    </header>
    <div id="chart_noun"></div>
</div>


<p>
    Indeed, the authors’ use of adjectives turns out to be more informative than that of nouns. In the analyzed novels,
    men were much more often <em>handsome</em>,
    <em>tall</em>, <em>bald</em>, <em>drunk</em>, <em>gay</em>, <em>lean</em>, <em>muscular</em>, <em>big</em>
    <em>good-looking</em> or <em>dead</em> than women. Women, by contrast, were much more often <em>beautiful</em>,
    <em>pregnant</em>, <em>pretty</em>, <em>safe</em>, <em>lovely</em>, <em>female</em>, <em>afraid</em>, <em>grateful</em>,
    <em>petite</em> or <em>blonde</em> than men. Notice that only two of these adjectives are exclusive to one
    gender: <em>pregnant</em> and <em>female</em>. While it is observed a few times with male subjects, this is probably the result
    of parsing errors. The other eighteen adjectives can refer to either of the sexes, in principle.
</p>

<p>
    Only, of course, they aren’t. The results contradict my earlier hypothesis that appearances are more important
    for women than for men. In fact, seven of the ten adjectives that co-occur most significantly with <em>he</em>
    refer to looks. What they do seem to show is a contrast between <em>active</em> men and <em>passive</em> women.
    Female characters stand on the sidelines more often &mdash; there they are <em>safe</em> from any dangers,
    <em>afraid</em> when things get violent, and <em>grateful</em> when all ends well. When it comes to appearances,
    authors single out their beauty (<em>beautiful</em>, <em>pretty</em>, <em>lovely</em>, <em>blonde</em>) or
    small stature (<em>petite</em>). Male characters, by contrast, seek center stage, where they end up <em>drunk</em>
    or <em>dead</em>. When their looks are described, this is done to stress how strong they are (<em>tall</em>,
    <em>lean</em>, <em>muscular</em>, <em>big</em>).
</p>

<div class="graph">
    <header><br/>
        <h2>
            s/he is &lt;adjective&gt;
        </h2>
    </header>
    <div id="chart_adj"></div>
</div>

<p>
    These findings are corroborated by the most frequent syntactic construction, where <em>he</em> or <em>she</em> are
    the subject of a verb. Again, passive emotions are most significantly correlated with the pronoun <em>she</em>:
    female characters <em>cry</em>, <em>scream</em>, <em>sob</em>, <em>gasp</em> and <em>blush</em> more often than their male
    counterparts. Indeed, the verb <em>feel</em> itself co-occurs much more often with <em>she</em> than with
    <em>he</em>. And whereas women <em>giggle</em>, men <em>chuckle</em>, <em>grin</em>, <em>grunt</em>, <em>growl</em>,
    <em>shout</em> and <em>roar</em> &mdash; all much more dominant verbs, associated with action or subjects in control
    of their emotions. The presence of <em>kill</em> confirms that male characters are typically caught up in the action,
    while the distribution of <em>wear</em> might indicate female characters are more often described by their clothing.
    Finally, the correlation between <em>he</em> and <em>kiss</em> proves that men do know love after all &mdash;
    although they let their female counterparts do the hugging.
</p>

<div class="graph">
    <header><br/>
        <h2>
            s/he &lt;verb&gt;
        </h2>
    </header>
    <div id="chart_verb"></div>
</div>

<p>
    It’s clear amateur literature is not free of stereotypes. The co-occurrence patterns of the pronouns <em>he</em>
    and <em>she</em> clearly show that male characters mostly go out into the action, while their female opponents
    display more passive emotions. Still, on the basis of this bird-view analysis,
    it’s hard to say how pervasive these stereotypes are. They might be restricted to a subset of authors or genres,
    such as fantasy or chick lit. Additionally, it would be interesting to see if stereotypes in books affected their
    popularity, and if novels by professional authors display the same tendencies.
    But those are all questions for another day.
</p>

<script>
  data = null;
  d3.csv('/data/gender_noun.csv', function(csv) {
    data = csv;
    // console.log("CSV", csv);
    var
        words = csv.map( function(d) { return d["verb"] }),
        data = csv
                      .map( function(d) {
                        var r = {
                          label: d["verb"],
                          he: parseFloat(d["he"]), // Math.log(parseFloat(d["he"])),
                          she: parseFloat(d["she"]), //Math.log(parseFloat(d["she"])),
                          //he_raw: parseFloat(d["he"]),
                          //she_raw: parseFloat(d["she"])
                        };
                        console.log(r);
                        return r;
                      })
                      //Require countries to have both values present
                      .filter(function(d) { return (!isNaN(d.he) && !isNaN(d.she)); }),
        // Extract the values for every country for both years in the dataset for the scale
        values = data
                      .map( function(d) { return d3.round(d.he, 5); })
                      .filter( function(d) { return d; } )
                      .concat(
                        data
                          .map( function(d) { return d3.round(d.she, 5); } )
                          .filter( function(d) { return d; } )
                      )
                      .sort(function (a,b) {return a - b;})
                      .reverse(),
        // Return true for countries without start/end values
        missing = function(d) { return !d.he || !d.she; },

        // Format values for labels
        label_format = function(value) { return d3.format(".1f")(d3.round(value, 1)); },

        font_size = 12,
        margin = 0,
        width = 800,
        // height = words.length * font_size*1.5 + margin,
        height = 400,

        chart = d3.select("#chart_noun").append("svg")
                   .attr("width", width)
                   .attr("height", height*1.9);


    // Scales and positioning
    var slope = d3.scale.ordinal()
                  .domain(values)
                  .rangePoints([margin, height]);

	//Go through the list of countries in order, adding additional space as necessary.
	var min_h_spacing = 1.2 * font_size, // 1.2 is standard font height:line space ratio
		previousY = 0,
		thisY,
		additionalSpacing;
	//Preset the Y positions (necessary only for the lower side)
	//These are used as suggested positions.
	data.forEach(function(d) {
		d.startY = slope(d3.round(d.he,5)); //slope(d3.round(d.he,1))
		d.endY = slope(d3.round(d.she,5));
	});
	//Loop over the higher side (right) values, adding space to both sides if there's a collision
	data
		.sort(function(a,b) {
			if (a.she == b.she) return 0;
			return (a.she < b.she) ? -1 : +1;
		})
		.forEach(function(d) {
			thisY = d.endY; //position "suggestion"
			additionalSpacing = d3.max([0, d3.min([(min_h_spacing - (thisY - previousY)), min_h_spacing])]);
	
			//Adjust all Y positions lower than this end's original Y position by the delta offset to preserve slopes:
			data.forEach(function(dd) {
				if (dd.startY >= d.endY) dd.startY += additionalSpacing;
				if (dd.endY >= d.endY) dd.endY += additionalSpacing;
			});
		
			previousY = thisY;
		});

	//Loop over the lower side (left) values, adding space to both sides if there's a collision
	previousY = 0;
	data
		.sort(function(a,b) {
			if (a.startY == b.startY) return 0;
			return (a.startY < b.startY) ? -1 : +1;
		})
		.forEach(function(d) {
			thisY = d.startY; //position "suggestion"
			additionalSpacing = d3.max([0, d3.min([(min_h_spacing - (thisY - previousY)), min_h_spacing])]);
	
			//Adjust all Y positions lower than this start's original Y position by the delta offset to preserve slopes:
			data.forEach(function(dd) {
				if (dd.endY >= d.startY) dd.endY += additionalSpacing;
				if (dd.label != d.label && dd.startY >= d.startY) dd.startY += additionalSpacing;
			});
			previousY = thisY;
		});

    // Countries
    var word = chart.selectAll("g.word")
                    .data( data )
                    .enter()
                    .append("g")
                    .attr("class", "word")
                    .classed("missing", function(d) { return missing(d); });

    word
      .on("mouseover", function(d,i) { return d3.select(this).classed("over", true); })
      .on("mouseout", function(d,i) { return d3.select(this).classed("over", false); });

    // ** Left column
    word
      .append("text")
      .classed("label start", true)
      .attr("x", 100)
      .attr("y", function(d) {return d.startY;})
      //.attr("y", function(d,i) { var rounded = d3.round(d.he, 1); return slope(rounded) })
      .attr("xml:space", "preserve")
      .style("font-size", font_size)
      .text(function(d) { return d.label+ " " + label_format(d.he); });

    // ** Right column
    word
      .append("text")
      .classed("label end", true)
      .attr("x", width-300)
      .attr("y", function(d) {return d.endY;})
      //.attr("y", function(d,i) { var rounded = d3.round(d.she, 1); return slope(rounded) })
      .attr("xml:space", "preserve")
      .style("font-size", font_size)
      .text(function(d) { return label_format(d.she) + " " + d.label; });

    // ** Slope lines
    word
      .append("line")
      .classed("slope", function(d) { return d.he || d.she; })
      .attr("x1", 110)
      .attr("x2", width-310)
      .attr("y1", function(d,i) {
        return d.he && d.she ? d.startY - font_size/2 + 2 : null;
      })
      .attr("y2", function(d,i) {
        return d.she && d.she ? d.endY - font_size/2 + 2 : null;
      });

    return chart;
  });
</script>


<script>
  data = null;
  d3.csv('/data/gender.csv', function(csv) {
    data = csv;
    // console.log("CSV", csv);
    var
        words = csv.map( function(d) { return d["verb"] }),
        data = csv
                      .map( function(d) {
                        var r = {
                          label: d["verb"],
                          he: parseFloat(d["he"]), // Math.log(parseFloat(d["he"])),
                          she: parseFloat(d["she"]), //Math.log(parseFloat(d["she"])),
                          //he_raw: parseFloat(d["he"]),
                          //she_raw: parseFloat(d["she"])
                        };
                        console.log(r);
                        return r;
                      })
                      //Require countries to have both values present
                      .filter(function(d) { return (!isNaN(d.he) && !isNaN(d.she)); }),
        // Extract the values for every country for both years in the dataset for the scale
        values = data
                      .map( function(d) { return d3.round(d.he, 5); })
                      .filter( function(d) { return d; } )
                      .concat(
                        data
                          .map( function(d) { return d3.round(d.she, 5); } )
                          .filter( function(d) { return d; } )
                      )
                      .sort(function (a,b) {return a - b;})
                      .reverse(),
        // Return true for countries without start/end values
        missing = function(d) { return !d.he || !d.she; },

        // Format values for labels
        label_format = function(value) { return d3.format(".1f")(d3.round(value, 1)); },

        font_size = 12,
        margin = 0,
        width = 800,
        // height = words.length * font_size*1.5 + margin,
        height = 360,

        chart = d3.select("#chart_adj").append("svg")
                   .attr("width", width)
                   .attr("height", height*1.9);


    // Scales and positioning
    var slope = d3.scale.ordinal()
                  .domain(values)
                  .rangePoints([margin, height]);

	//Go through the list of countries in order, adding additional space as necessary.
	var min_h_spacing = 1.2 * font_size, // 1.2 is standard font height:line space ratio
		previousY = 0,
		thisY,
		additionalSpacing;
	//Preset the Y positions (necessary only for the lower side)
	//These are used as suggested positions.
	data.forEach(function(d) {
		d.startY = slope(d3.round(d.he,5)); //slope(d3.round(d.he,1))
		d.endY = slope(d3.round(d.she,5));
	});
	//Loop over the higher side (right) values, adding space to both sides if there's a collision
	data
		.sort(function(a,b) {
			if (a.she == b.she) return 0;
			return (a.she < b.she) ? -1 : +1;
		})
		.forEach(function(d) {
			thisY = d.endY; //position "suggestion"
			additionalSpacing = d3.max([0, d3.min([(min_h_spacing - (thisY - previousY)), min_h_spacing])]);

			//Adjust all Y positions lower than this end's original Y position by the delta offset to preserve slopes:
			data.forEach(function(dd) {
				if (dd.startY >= d.endY) dd.startY += additionalSpacing;
				if (dd.endY >= d.endY) dd.endY += additionalSpacing;
			});

			previousY = thisY;
		});

	//Loop over the lower side (left) values, adding space to both sides if there's a collision
	previousY = 0;
	data
		.sort(function(a,b) {
			if (a.startY == b.startY) return 0;
			return (a.startY < b.startY) ? -1 : +1;
		})
		.forEach(function(d) {
			thisY = d.startY; //position "suggestion"
			additionalSpacing = d3.max([0, d3.min([(min_h_spacing - (thisY - previousY)), min_h_spacing])]);

			//Adjust all Y positions lower than this start's original Y position by the delta offset to preserve slopes:
			data.forEach(function(dd) {
				if (dd.endY >= d.startY) dd.endY += additionalSpacing;
				if (dd.label != d.label && dd.startY >= d.startY) dd.startY += additionalSpacing;
			});
			previousY = thisY;
		});

    // Countries
    var word = chart.selectAll("g.word")
                    .data( data )
                    .enter()
                    .append("g")
                    .attr("class", "word")
                    .classed("missing", function(d) { return missing(d); });

    word
      .on("mouseover", function(d,i) { return d3.select(this).classed("over", true); })
      .on("mouseout", function(d,i) { return d3.select(this).classed("over", false); });

    // ** Left column
    word
      .append("text")
      .classed("label start", true)
      .attr("x", 100)
      .attr("y", function(d) {return d.startY;})
      //.attr("y", function(d,i) { var rounded = d3.round(d.he, 1); return slope(rounded) })
      .attr("xml:space", "preserve")
      .style("font-size", font_size)
      .text(function(d) { return d.label+ " " + label_format(d.he); });

    // ** Right column
    word
      .append("text")
      .classed("label end", true)
      .attr("x", width-300)
      .attr("y", function(d) {return d.endY;})
      //.attr("y", function(d,i) { var rounded = d3.round(d.she, 1); return slope(rounded) })
      .attr("xml:space", "preserve")
      .style("font-size", font_size)
      .text(function(d) { return label_format(d.she) + " " + d.label; });

    // ** Slope lines
    word
      .append("line")
      .classed("slope", function(d) { return d.he || d.she; })
      .attr("x1", 110)
      .attr("x2", width-310)
      .attr("y1", function(d,i) {
        return d.he && d.she ? d.startY - font_size/2 + 2 : null;
      })
      .attr("y2", function(d,i) {
        return d.she && d.she ? d.endY - font_size/2 + 2 : null;
      });

    return chart;
  });
</script>

<script>
  data = null;
  d3.csv('/data/gender.csv', function(csv) {
    data = csv;
    // console.log("CSV", csv);
    var
        words = csv.map( function(d) { return d["verb"] }),
        data = csv
                      .map( function(d) {
                        var r = {
                          label: d["verb"],
                          he: parseFloat(d["he"]), // Math.log(parseFloat(d["he"])),
                          she: parseFloat(d["she"]), //Math.log(parseFloat(d["she"])),
                          //he_raw: parseFloat(d["he"]),
                          //she_raw: parseFloat(d["she"])
                        };
                        console.log(r);
                        return r;
                      })
                      //Require countries to have both values present
                      .filter(function(d) { return (!isNaN(d.he) && !isNaN(d.she)); }),
        // Extract the values for every country for both years in the dataset for the scale
        values = data
                      .map( function(d) { return d3.round(d.he, 5); })
                      .filter( function(d) { return d; } )
                      .concat(
                        data
                          .map( function(d) { return d3.round(d.she, 5); } )
                          .filter( function(d) { return d; } )
                      )
                      .sort(function (a,b) {return a - b;})
                      .reverse(),
        // Return true for countries without start/end values
        missing = function(d) { return !d.he || !d.she; },

        // Format values for labels
        label_format = function(value) { return d3.format(".1f")(d3.round(value, 1)); },

        font_size = 12,
        margin = 0,
        width = 800,
        // height = words.length * font_size*1.5 + margin,
        height = 360,

        chart = d3.select("#chart_verb").append("svg")
                   .attr("width", width)
                   .attr("height", height*1.9);


    // Scales and positioning
    var slope = d3.scale.ordinal()
                  .domain(values)
                  .rangePoints([margin, height]);

	//Go through the list of countries in order, adding additional space as necessary.
	var min_h_spacing = 1.2 * font_size, // 1.2 is standard font height:line space ratio
		previousY = 0,
		thisY,
		additionalSpacing;
	//Preset the Y positions (necessary only for the lower side)
	//These are used as suggested positions.
	data.forEach(function(d) {
		d.startY = slope(d3.round(d.he,5)); //slope(d3.round(d.he,1))
		d.endY = slope(d3.round(d.she,5));
	});
	//Loop over the higher side (right) values, adding space to both sides if there's a collision
	data
		.sort(function(a,b) {
			if (a.she == b.she) return 0;
			return (a.she < b.she) ? -1 : +1;
		})
		.forEach(function(d) {
			thisY = d.endY; //position "suggestion"
			additionalSpacing = d3.max([0, d3.min([(min_h_spacing - (thisY - previousY)), min_h_spacing])]);

			//Adjust all Y positions lower than this end's original Y position by the delta offset to preserve slopes:
			data.forEach(function(dd) {
				if (dd.startY >= d.endY) dd.startY += additionalSpacing;
				if (dd.endY >= d.endY) dd.endY += additionalSpacing;
			});

			previousY = thisY;
		});

	//Loop over the lower side (left) values, adding space to both sides if there's a collision
	previousY = 0;
	data
		.sort(function(a,b) {
			if (a.startY == b.startY) return 0;
			return (a.startY < b.startY) ? -1 : +1;
		})
		.forEach(function(d) {
			thisY = d.startY; //position "suggestion"
			additionalSpacing = d3.max([0, d3.min([(min_h_spacing - (thisY - previousY)), min_h_spacing])]);

			//Adjust all Y positions lower than this start's original Y position by the delta offset to preserve slopes:
			data.forEach(function(dd) {
				if (dd.endY >= d.startY) dd.endY += additionalSpacing;
				if (dd.label != d.label && dd.startY >= d.startY) dd.startY += additionalSpacing;
			});
			previousY = thisY;
		});

    // Countries
    var word = chart.selectAll("g.word")
                    .data( data )
                    .enter()
                    .append("g")
                    .attr("class", "word")
                    .classed("missing", function(d) { return missing(d); });

    word
      .on("mouseover", function(d,i) { return d3.select(this).classed("over", true); })
      .on("mouseout", function(d,i) { return d3.select(this).classed("over", false); });

    // ** Left column
    word
      .append("text")
      .classed("label start", true)
      .attr("x", 100)
      .attr("y", function(d) {return d.startY;})
      //.attr("y", function(d,i) { var rounded = d3.round(d.he, 1); return slope(rounded) })
      .attr("xml:space", "preserve")
      .style("font-size", font_size)
      .text(function(d) { return d.label+ " " + label_format(d.he); });

    // ** Right column
    word
      .append("text")
      .classed("label end", true)
      .attr("x", width-300)
      .attr("y", function(d) {return d.endY;})
      //.attr("y", function(d,i) { var rounded = d3.round(d.she, 1); return slope(rounded) })
      .attr("xml:space", "preserve")
      .style("font-size", font_size)
      .text(function(d) { return label_format(d.she) + " " + d.label; });

    // ** Slope lines
    word
      .append("line")
      .classed("slope", function(d) { return d.he || d.she; })
      .attr("x1", 110)
      .attr("x2", width-310)
      .attr("y1", function(d,i) {
        return d.he && d.she ? d.startY - font_size/2 + 2 : null;
      })
      .attr("y2", function(d,i) {
        return d.she && d.she ? d.endY - font_size/2 + 2 : null;
      });

    return chart;
  });
</script>

