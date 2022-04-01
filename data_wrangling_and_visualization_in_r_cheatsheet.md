# Data Wrangling and Visualization in R

Weilin Zhou



Referred to Prof's lecture notes and online material to create a cheatsheet for G5293 course, which is mainly aimed at introducing useful plots 

https://hbctraining.github.io/Intro-to-R-flipped/cheatsheets/data-visualization-2.1.pdf
http://www.sthda.com/english/wiki/be-awesome-in-ggplot2-a-practical-guide-to-be-highly-effective-r-software-and-data-visualization#one-variable-discrete 

## Introduction

R is a good tool to apply data cleaning and data visualization.
### Prerequisites:
Useful packages inside R may include:

```r
library(tidyverse) 
library(dplyr)
# provide simple functions to the most common data manipulation tasks
# 'ggplot2' is included in tidyverse
```

### Import data:

```r
flights_data <- nycflights13::flights #dataset in package
data_set <- read.csv("filepath/filename.csv") #outer source
#head(flights_data)
```
### Data Wrangling:

It is important to keep dataset 'clean' when visualizing data, which can be done before the visualization or adjusted in the process of visualization.

* Useful functions to solve data cleaning:
  + filter(): select observations based on their values
  + arrange(): reorder observations
  + select(): select variables based on names
  + mutate(): add variables as functions of existing variables
  + group_by(): operate groupwise by names
  + summarize(): collapse many values down to a single
  + drop_na(): drop off NA values 
  + top_n(): select and order top n entries
  + distinct(): remove duplicate rows
  + left/right/inner/full_join(): types of joining data
  

* Two more useful steps of tidying:
  + pivot_longer(): increase number of rows, which can be used to tidy datasets optimized for data entry rather than analysis
  + pivot_wider(): increase number of column, which is useful to create summary tables for presentation data

## Data Visualization:

### Grammar of Graphics:
Formula of building a graph:


```r
ggplot(data = df)+
  geom_function(mapping=aes(x=x=,y,y),stat=,Position=)+
  coordinate_function+
  facet_function+
  scale_function+
  theme_function+
  labs(title="Graph title",
       x="X-axis name",
       y="Y-axis name")
```

* Layers:
  + Dataset
  + Aesthetic mapping: x-loc/y-loc/shape/size/fill/color/alpha/group
  + Geom: point/bar/boxplot/histogram/density/hex
  + Stat: bin/boxplot/identity/density
  + Position: identity/jitter/dodge/stack
* Scale:
  + scale_*_date()
  + scale_*_continuous()
  + scale_*_identity()
  + scale_color_manual()
  + scale_fill_viridis_c()
* Coord (Only 1 can be used):
  + coord_polar(): use polar coordinate system
  + coord_fixed(): fixed scale coordinate system, ratio = y-axis/x-axis
* Facet (Only 1 can be used):
  + facet_grid(): form a matrix of panels defined by row and column 
  + facet_wrap(): wrap 1d sequence of panels into 2d
* Theme (Only 1 can be used):
  + theme_grey(): default theme and have a grey plot background
  + theme_bw(): use a white background and thin grey grid lines
  + theme_linedraw(): with only black lines of various widths on white backgrounds
  + theme_minimal(): a minimalistic theme with no background annotations
  + theme_void(): completely empty theme

### Continuous Variables:

* Types of categorical data:
  + nominal: no fixed category order
  + ordinal: fixed category order

#### One variable:

1. Histogram:
Show the frequency of values of a variable

```r
ggplot(df, aes(x=x))+
    geom_histogram()  # Basic histogram
or  geom_histogram(binwidth=1) # Change the width of bins
or  geom_histogram(breaks=seq(30,110,10)) 
# Another way to change width of bins
or  geom_histogram(color="black",fill="white",linetype="dashed") 
# Change colors/fills/linetypes
or  geom_histogram(aes(y = cumsum(..count..))) 
# create cumulative frequency histogram

Additions:
    +geom_density(color="red") # Add density curve 
    +stat_function(fun=dnorm,args=list(mean=0,sd=1),color="red",lwd=1.5) 
    #Add normal curve    
```

2. Area Plot:
Represnet the evolution of a numeric variable

```r
ggplot(df,aes(x=x))+
  geom_area(aes(fill=y),stat="bin",alpha=0.6) 
#fill color by y-var
```

3. Dot Plot:

```r
ggplot(df,aes(x=x))+
  geom_dotplot(aes(fill=y)) 
#fill color by y-var
```

4. Cleveland dot plot:


```r
ggplot(df,aes(x=x,y=fct_reorder(y,x),color = z))+ 
  # color is used for large number of categories
  geom_point(color="blue")+
  theme_linedraw()+
  facet_grid(.~reorder(y,-x,median),scales = "free_y",space="free_y") 
  # used for faceting
  # scales: determine whether the scales are shared across all facets
  # sapce: determine whether all panels have the same size
```

#### Two Variables:

1. Scatterplot (Continuous X, Coutinuous Y):



```r
ggplot(ddf,aes(x,y))+
  geom_point(alpha=0.5,size=1)+
  geom_smooth(method=lm) 
#add a regression line on a scatterplot

#alpha: adjust transparency of point
#size: adjust size of point
```

2. Boxplots (Discrete X, Continuous Y):

A measure of how well distributed is the data in a dataset

```r
ggplot(df,aes(x=x,y=y))+
    geom_boxplot() # Basic boxplot
or  geom_boxplot() + coord_flip() # Rotate the boxplot
or  geom_boxplot(outlier.color="red",outlier.shape=5) 
  # Make adjustments of outliers
```

3. Violin plot (Discrete X, Continuous Y):

Similar to boxplot, but show the kerneal probability density of data

```r
ggplot(df,aes(x=x,y=y))+
  geom_violin(trim=FALSE)+
  stat_summary(fun.data="mean_sdl",fun.args=list(mult=1),geom="pointrange",color="blue")
#add mean points (+/- SD)
```

4. Ridgeline plot:


```r
library(ggridges)
ggplot(df, aes(x=x,y=y))+
  geom_density_ridges(fill="blue",alpha=0.5)
# fill: change inside color
# alpha: specify the level of transparency
```

5. Parallel Coordinates:


```r
library(GGally)
ggparcoord(df,columns=num:num,groupColumn=num,
           scale='globalminmax/uniminmax/std/center',
           showPoints = TRUE/FALSE,
           title="Title",
           alphaLines = 0.3)
  #Showpoints: add dots
  #Alphalines: add transparency
```

* Scale Options:
  + globalminmax: No scaling
  + uniminmax: Stadardize to min=0 and max=1
  + std: Normalize univariately (Subtract mean & divide by sd)
  + center: Standardize and center variables
  
6. Bar plot:


```r
ggplot(df,aes(x=x))+
  geom_bar(fill="color")
```

7. Heatmaps (Continuous bivariate distribution):


```r
ggplot(df,aes(x=x,y=y))+
  geom_bin2d(binwidth=c(num,num))
  # Square heatmap
  
or geom_hex(binwidth=c(num,num))
  # Hex heatmap
```

8. 2d density estimate (Continuous bivariate distribution):

Used to add 2d density estimate to a scatter plot

```r
ggplot(df, aes(x=x,y=y))+
  geom_point()+
  geom_density_2d()
```

### Additional Interesting plots:

1. Principal Components Analysis & Biplot:
Goal: explain most of the variability in a dataset with fewer variables than the original dataset


```r
library(redav)
pca <- prcomp(df[num:num],scale.=TRUE)

biplot(results,scale=0) #Basic biplot

draw_biplot(df,"var",project=FALSE) #Biplot with calibrated axis
```


```r
#Calculate total variance explained by each principal component
var_explained <- results$sdev^2 / sum(results$sdev^2)
```

2. Mosaic Plots:
A visual representation of the association between two or more categorical variables.

```r
library(vcd)
mosaic(~Var1+Var2+..+Varn, data=df,
       direction = c("v/h","h/v")  
       main = "Title name",
       shade = TRUE/FALSE, 
       legend = TRUE/FALSE)
#direction: layout direction of each variable
#shade: whether to color the figure
#legend: whether to display a legend for data
```

3. Interactive plot:

```r
library(plotly)

plotly::plot_ly(df,x=x,y=y,color=z,text=w)%>%
  add_markers()
```

4. Alluvial Plot:
Group categorical data into flows that can easily be traced in the diagram.

```r
library(ggalluvial)
ggplot(df,aes(axis1=Class1,axis2=Class2,y=Freq))+
  geom_alluvium(color="blue")+
  geom_stratum()+
  geom_text(stat = "stratum",aes(label=paste(after_stat(stratum),"\n",after_stat(count))))+
  scale_x_discrete(limits = c("Class1","Class2"))
```
