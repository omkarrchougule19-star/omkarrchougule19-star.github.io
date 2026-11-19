---
name: Bigfoot Sightings with Altair
tools: [Python, Altair, vega-lite]
image: assets/pngs/cars.png
description: Two Altair visualizations of BFRO Bigfoot sighting reports, one of them interactive.
custom_js:
  - vega.min
  - vega-lite.min
  - vega-embed.min
  - justcharts
---

# Bigfoot Sightings: When and Where

Both plots use the Bigfoot Field Researchers Organization (BFRO) report dataset. It has about 4,700 reported sightings, each with a date, season, US state and a classification. Class A means a clear first-hand sighting, and Class B means a less certain encounter, such as hearing sounds or finding tracks.

## Plot 1: Reports per year by classification

<vegachart schema-url="{{ site.baseurl }}/assets/json/bigfoot_by_year.json" style="width: 100%"></vegachart>

**What it shows.** The number of Bigfoot reports per year from 1960 to 2018, with separate lines for Class A and Class B reports. Reports grow slowly through the 1990s, spike sharply in the early-to-mid 2000s, and then decline. Class B reports overtake Class A after about 2005.

**Encodings.** Year is a temporal variable on the x-axis, and the number of reports is a quantitative variable on the y-axis. Position along a common axis is the most accurate channel for comparing amounts, so it carries the main data. The line mark shows the trend over time. Points mark each actual yearly value, and tooltips show the exact year, class and count.

**Color.** Color encodes the report classification, a nominal variable with two values. I used a categorical scheme from the ColorBrewer "Dark2" palette: teal for Class A and purple for Class B. These hues are clearly different, have similar lightness so neither line looks more important, and stay distinguishable for the most common forms of color blindness. A sequential or diverging map would wrongly suggest that one class is "more" than the other.

**Transformations.** In the notebook I parsed the `date` column into pandas timestamps and extracted the year. I dropped rows with no date (about 950) for this plot only and kept only Class A and Class B, because Class C has just 30 second-hand reports. I limited the range to 1960–2018 because earlier years have only a few reports and the dataset ends in 2018. Finally I grouped by year and class to count reports, then converted the year back to a timestamp so Altair treats it as temporal.

## Plot 2: Seasons and states (interactive)

<vegachart schema-url="{{ site.baseurl }}/assets/json/bigfoot_season_state.json" style="width: 100%"></vegachart>

**What it shows.** The left panel shows the total number of reports for each season. The right panel ranks the 15 states with the most reports for whatever is currently selected. With nothing selected, Washington, California and Florida lead, and summer is clearly the busiest season.

**Encodings.** Both panels are bar charts. Bar length encodes a quantitative count, and the nominal categories (season or state) sit on the other axis. Seasons follow calendar order (Spring, Summer, Fall, Winter) so they read as a cycle. States are sorted by count from highest to lowest, which makes the ranking easy to read. Tooltips show exact counts.

**Color.** Color encodes season, a nominal variable, using a categorical palette with intuitive associations: green for spring, yellow for summer, orange for fall and blue for winter. This makes the season bars quick to identify. Unselected seasons turn light gray, so the color also shows what is currently selected. The state bars use a single neutral dark gray, because color would only repeat the length encoding and add no information.

**Transformations.** I removed reports with an "Unknown" season, kept Class A and B, and aggregated in pandas to counts per state × season × classification. This shrinks the exported JSON from about 4,700 long text records to a small table. The interactive filtering, the per-state sum, and the top-15 ranking (a window `rank` transform) are done in Vega-Lite so they update live with the selection.

## Interactivity

Plot 2 has two linked interactions. **Clicking a season bar** (shift-click to pick several) filters the state ranking to reports from that season. **A "Report class" dropdown** limits both panels to Class A, Class B or all reports. Together they let a reader ask questions that one static chart can't answer, such as "Which states lead in winter?" or "Does the state ranking change if I count only clear first-hand sightings?" For example, selecting Winter shows how the ranking reshuffles for that season. The gray-out effect also makes the current filter obvious, so the reader always knows what subset the right panel shows. Plot 1 has hover tooltips for exact values.

## The Data & Analysis

<div class="left">
{% include elements/button.html link="https://github.com/UIUC-iSchool-DataViz/is445_data/blob/main/bfro_reports_fall2022.csv" text="The Data" %}
</div>

<div class="right">
{% include elements/button.html link="https://github.com/omkarrchougule19-star/omkarrchougule19-star.github.io/blob/main/python_notebooks/bigfoot_altair.ipynb" text="The Analysis" %}
</div>
