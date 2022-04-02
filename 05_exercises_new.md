---
title: 'Weekly Exercises #5'
author: "Cynthia Zhang"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels and alt text.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.
  

```r
garden_ly <- garden_harvest %>%
  mutate(weight_pound = weight * 0.00220462) %>%
  filter(vegetable %in% c("tomatoes")) %>%
  group_by(variety) %>%
  summarize(weight_tot = sum(weight_pound), first_harvest = min(date)) %>%
  arrange(first_harvest) %>%
  ggplot(aes(x = weight_tot, fct_reorder(variety, first_harvest))) +
  geom_col() +
  labs(x = "Weight(pounds)",
       y = "Varieties of Tomoato",
       title = "Total Harvested Weight of Different Varieties of Tomato")

ggplotly(garden_ly)
```

```{=html}
<div id="htmlwidget-7be9a758756e3427d3ab" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-7be9a758756e3427d3ab">{"x":{"data":[{"orientation":"v","width":[32.39468628,24.99377694,24.9232291,34.00846812,15.71232674,65.67342518,26.32536742,15.0244853,26.71778978,15.8071254,15.64618814,51.61235882],"base":[0.55,1.55,2.55,3.55,4.55,5.55,6.55,7.55,8.55,9.55,10.55,11.55],"x":[16.19734314,12.49688847,12.46161455,17.00423406,7.85616337,32.83671259,13.16268371,7.51224265,13.35889489,7.9035627,7.82309407,25.80617941],"y":[0.9,0.9,0.9,0.9,0.9,0.9,0.9,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999,0.899999999999999],"text":["weight_tot: 32.39469<br />fct_reorder(variety, first_harvest): grape","weight_tot: 24.99378<br />fct_reorder(variety, first_harvest): Big Beef","weight_tot: 24.92323<br />fct_reorder(variety, first_harvest): Bonny Best","weight_tot: 34.00847<br />fct_reorder(variety, first_harvest): Better Boy","weight_tot: 15.71233<br />fct_reorder(variety, first_harvest): Cherokee Purple","weight_tot: 65.67343<br />fct_reorder(variety, first_harvest): Amish Paste","weight_tot: 26.32537<br />fct_reorder(variety, first_harvest): Mortgage Lifter","weight_tot: 15.02449<br />fct_reorder(variety, first_harvest): Jet Star","weight_tot: 26.71779<br />fct_reorder(variety, first_harvest): Old German","weight_tot: 15.80713<br />fct_reorder(variety, first_harvest): Black Krim","weight_tot: 15.64619<br />fct_reorder(variety, first_harvest): Brandywine","weight_tot: 51.61236<br />fct_reorder(variety, first_harvest): volunteers"],"type":"bar","marker":{"autocolorscale":false,"color":"rgba(89,89,89,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":113.24200913242},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Total Harvested Weight of Different Varieties of Tomato","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-3.283671259,68.957096439],"tickmode":"array","ticktext":["0","20","40","60"],"tickvals":[0,20,40,60],"categoryorder":"array","categoryarray":["0","20","40","60"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Weight(pounds)","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,12.6],"tickmode":"array","ticktext":["grape","Big Beef","Bonny Best","Better Boy","Cherokee Purple","Amish Paste","Mortgage Lifter","Jet Star","Old German","Black Krim","Brandywine","volunteers"],"tickvals":[1,2,3,4,5,6,7,8,9,10,11,12],"categoryorder":"array","categoryarray":["grape","Big Beef","Bonny Best","Better Boy","Cherokee Purple","Amish Paste","Mortgage Lifter","Jet Star","Old German","Black Krim","Brandywine","volunteers"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Varieties of Tomoato","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"1464222f44ee3":{"x":{},"y":{},"type":"bar"}},"cur_data":"1464222f44ee3","visdat":{"1464222f44ee3":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```

  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
late_departure <- small_trains %>%
  select(departure_station, journey_time_avg, total_num_trips, num_late_at_departure) %>%
  group_by(departure_station, journey_time_avg) %>%
  summarize(prop_late_departure = num_late_at_departure / total_num_trips) %>%
  ggplot(aes(x = journey_time_avg, y = prop_late_departure, group = departure_station)) +
  geom_jitter() +
  labs(title = "Association between journey time and proportion of departuring late",
       subtitle = "Departure Station: {closest_state}",
       x = "Time of Journey",
       y = "Proportion of Departing Late") +
  transition_states(departure_station, 
                    transition_length = 2, 
                    state_length = 1) +
  exit_shrink() +
  enter_recolor(color = "lightblue") +
  exit_recolor(color = "lightblue")

animate(late_departure, duration = 20, nframes = 400)
```

![](05_exercises_new_files/figure-html/unnamed-chunk-2-1.gif)<!-- -->

```r
anim_save("train.gif")
```

> I create this animation to investigate the relationship between the total time of journey and the likelihood of departing late. From the animation, we can observe that for each station, most of the dots are scatter at the bottom of the graph which means most of the journeies have pretty low proportion of being late. But as the time of journey getting longer, some stations show a trend of more likely departing late which is shown as some dots reach to the top of the graph. Also, different stations have different performance. For some stations, their dots are mostly scattered at the bottom of the graph so that the trains departing from those stations are unlikely being late. But for some stations, their dots are mostly scatter at the top of the graph so that the trains departing from those stations are more likely being late.

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. I have filtered the data to the tomatoes and find the *daily* harvest in pounds for each variety. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0. 
  You should do the following:
  * For each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each variety and arranged (HINT: `fct_reorder()`) from most to least harvested weights (most on the bottom).  
  * Add animation to reveal the plot over date. Instead of having a legend, place the variety names directly on the graph (refer back to the tutorial for how to do this).


```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>%
  ungroup() %>% 
  complete(variety, 
           date, 
           fill = list(daily_harvest_lb = 0)) %>%
  group_by(variety) %>%
  mutate(cum_weight = cumsum(daily_harvest_lb)) %>%
  ggplot(aes(x = date, y = cum_weight, fill = fct_reorder(variety, desc(daily_harvest_lb), sum))) +
  geom_area() +
  labs(title = "Cumulative harvest (lb)", 
       subtitle = "Date: {frame_along}",
       x = "",
       y = "",
       color = "variety") +
  theme(legend.position = "none") +
  transition_reveal(date)
```

![](05_exercises_new_files/figure-html/unnamed-chunk-3-1.gif)<!-- -->

```r
anim_save("tomato_harvest.gif")
```


## Maps, animation, and movement!

  4. Map Lisa's `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.


```r
mallorca_map <- get_stamenmap(
    bbox = c(left = 1.9514, bottom = 39.0971, right = 4.105 , top = 40.0981), 
    maptype = "terrain",
    zoom = 10
)
ggmap(mallorca_map) +
  geom_point(data = mallorca_bike_day7, 
            aes(x = lon, y = lat),
            color = "red",
            size = 2) +
  geom_path(data = mallorca_bike_day7, 
            aes(x = lon, y = lat, color = ele),
            size = 2) +
  theme_map() +
  labs(title = "Cycling Track", 
       subtitle = "time: {frame_along}",
       x = "",
       y = "") +
  transition_reveal(time)
```

![](05_exercises_new_files/figure-html/unnamed-chunk-4-1.gif)<!-- -->

```r
anim_save("cycling.gif")
```

> I prefer the animation. Because from the animation, I can clearly and easily track the path of cycling as time pass. Compared to the static map, I can observe where the cycling starts and where it ends from the animation.
  
  5. In this exercise, you get to meet Lisa's sister, Heather! She is a proud Mac grad, currently works as a Data Scientist where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files putting them in swim, bike, run order (HINT: `bind_rows()`), 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
panama_ironman <- bind_rows(panama_swim, panama_bike, panama_run) %>%
  select(event, lon, lat, time) %>%
  group_by(event)


panama_map <- get_stamenmap(
    bbox = c(left = -79.6392, bottom = 8.8891, right = -79.4521 , top = 8.9914), 
    maptype = "terrain",
    zoom = 13
)

ggmap(panama_map) +
  geom_point(data = panama_ironman, 
            aes(x = lon, y = lat, color = event),
            size = 2) +
  geom_path(data = panama_ironman, 
            aes(x = lon, y = lat, color = event),
            size = 1) +
  theme_map() +
  labs(title = "Panama Ironman Track", 
       subtitle = "time: {frame_along}",
       x = "",
       y = "") +
  transition_reveal(time)
```

![](05_exercises_new_files/figure-html/unnamed-chunk-5-1.gif)<!-- -->

```r
anim_save("ironman.gif")
```
  
## COVID-19 data

  6. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for the the 15th of each month. So, filter only to those dates - there are some lubridate functions that can help you do this.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")

covid_now <- covid19 %>%
  arrange(desc(date)) %>%
  group_by(state) %>%
  mutate(rownumber = 1:n()) %>%
  mutate(state = str_to_lower(`state`))

covid_pop <- covid_now %>%
  left_join(census_pop_est_2018,
            by = c("state" = "state")) %>%
  mutate(cases_10000 = (cases/est_pop_2018)*10000)

covid_pop %>%
  filter(day(date) == 15) %>%
  ggplot() +
  geom_map(map = states_map,
           aes(map_id = state,
               fill = cases_10000,
               group = date)) +
  expand_limits(x = states_map$long, y = states_map$lat) +
  labs(title = "COVID cases per 10000 people",
       subtitle = "Date: {closest_state}") +
  scale_fill_viridis_c(option = "magma", direction = -1) +
  theme_map() +
  transition_states(date)
```

![](05_exercises_new_files/figure-html/unnamed-chunk-6-1.gif)<!-- -->

```r
anim_save("covid_case.gif")
```

> From the animation, I can observe that covid cases first appears in the west coast then all the states report cases and keep increasing. The number of cases grows faster in the states in the middle and states in the southeast coast. Till now, states up north and states in the southeast also have the most amount of covid cases. 

## Your first `shiny` app (for next week!)

  7. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. You should create a new project for the app, separate from the homework project. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' daily number of COVID cases per 100,000 over time. The x-axis will be date. You will have an input box where the user can choose which states to compare (`selectInput()`), a slider where the user can choose the date range, and a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
 

```r
covid_daily_100000 <- covid19 %>%
  mutate(state = str_to_lower(`state`)) %>%
      left_join(census_pop_est_2018,
                by = c("state" = "state")) %>%
  group_by(state) %>%
  filter(est_pop_2018 != "NA") %>%
  mutate(cases_100000 = (cases/est_pop_2018)*100000)
```

  
Put the link to your app here: 

https://zhangcynthia.shinyapps.io/covidcases/.

## GitHub link

  8. Below, provide a link to your GitHub repo with this set of Weekly Exercises. 
  
https://github.com/zhangcynthia/Exercise5

**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
