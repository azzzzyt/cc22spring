#  Memos, checklist and usful links: experience of statistical graphic

Jiayuan Zhou, Wenqi Yu

Motivation of community contribution: This community contribution aim to share experience and create a short checklist for effective process, which show like a blog that people can search online. The reason why we want to do that is, people will think work for statistical graphic project might be a easy way. Nowadays, people can find the advertisement of transfer to data analysis and get more offer because of data analysis on the Internet, and most of posts describe the process as same as everyone can do that. However, student who really learn the course (like STAT5293 statistical graphics) will know that there are lots of work behind the simple simple, beautiful and easy to understand pictures. Therefore we want to write this and share to not only student who take this course, and also the people who may get involved in the future. Also, we chose to do it as a group because all of our content was based on our experience and some mistakes we had made. The information provided by two people would be more comprehensive than one person's, especially since one of them was still systematically exposed to the r language for the first time this semester, which made picking out programming-type problems would be more introductory. In addition, we also check the gaps of our past knowledge in the process of writing, which is equivalent to reviewing the contents of the class again. So I think what we did has some meaning and value. However, because of time, we didn't cover the later chapters with content about r project and git, so if we have a chance afterwards, we might add it in.


Part 1 Communication

Like the first step of finish Problem Sets is read the question, believe the first thing to confirm is the project requirements when accepting a project and being assigned a task. To complicate matters, sometimes an extra help is needed to understand the meaning of the data you are doing -- especially when you need to work on areas you have never touched. After all, the "Explanation Document" can only give you a rough introduction. If you need more accurate content, you need to search the Internet yourself, or book a meeting with the data provider, which is similar to what we did when we get trouble on doing Problem Sets: ask on ED discussion or meet with professor.

Also, the project is not a one-person battlefield, and teamwork is more beneficial to the project. Therefore, how to communicate with my teammates, we think it is also a key point. Based on our experience, we feel that it is very meaningful to have multiple progress reports after assigning tasks. But how to report, how to let your teammates know what you are doing right, or help find mistakes, we think this is a difficult point. At present, we think that a better solution (mainly for homework communication for two-person groups) is to share the requirements after reading the requirements together. After that, the first meeting is scheduled according to their respective time. The main purpose is to communicate your own drafts or progress, and explain the processing of important data to the other party, such as merging cluster data, marking data outliers, etc. Questions about details can be discussed at the same time, and questions can be saved to ask the professor if necessary. Subsequent meetings can repeat this process until the final version is completed. Of course, in the meantime, discussions about ambiguous results and professors are also needed to avoid questionable results.

In addition, for some business analysis, I have recently been reading *The Minto Pyramid Principle Logic in Writing Thinking* and *Storytelling with Data*, which I find very interesting as supplementary reading for business analysis.

Then, when these investigations of background knowledge about the data and other preliminary preparations are sufficient, further analysis of the data itself can be carried out.

Part 2 Analysis and Visualization Process

`ggplot2` and `tidyverse` are the package that is extremely simple to use for everyone. When we get a data set, we need to take a cursory scan and think about what our needs are and what do we want. Before coding, we need clean data. Popular methods are deleting the messy data, dealing with missing values, adjusting format, and unified name.

For example, in Problem Set2, we cluster the separated categories ("Vegerables A\~E", "Vegerables F\~P" etc.) and unified name as "Vegetables". Here we apply 'tidyverse' and regular expression to make code more readable, since we find the base R in team project quite hard to understand.


```r
# combine.2density <- two.density %>%
#   mutate_if(grepl("Vegetables",.), ~replace(., grepl('Vegetables', .), "Vegetables")) %>%
#   mutate_if(grepl("Fruit",.), ~replace(., grepl('Fruit', .), "Fruit"))
```

Then we can follow the instruction to draw the plot.

R beginner might use base R when plot results, but for complex output, `ggplot2` is more powerful tools. However, `ggplot2` is not such easier for beginner, so we list some problems we met before based on useful basic function of `ggplot2`.

First, in the process of editing and drawing, if we draw a various chart with cumulative conditions, it is a line-by-line independent running function in python. But in `ggplot2`, it is **"+"**. This is different with most other programming languages, but there is still a similar "modular" design. For more professional explanation, check [stackoverflow page](https://stackoverflow.com/questions/40450904/how-is-ggplot2-plus-operator-defined).

So, if get trouble or find bugs in the final output, we suggest three solutions (some may like how to debug):

a.  For the codes, removed each part you think have problems, and observed how the graph would change;

b.  Browsed stackoverflow (or others) and found the similar solutions from the source code, and consider how to improve yours;

c.  The `ggplot2` is inspired by *The Grammar of Graphics book*, which summarizes the drawing process into data, transformation, scale, coordinates, elements, guides, display, etc. A series of independent steps that are combined to achieve personalized statistical plots. The `ggplot2` is very subtle, we think it is more concise and intuitive than the drawing logic of python's matplotlib in statistical grapics.

Second, It is very convenient to use the *facet_wrap* and *facet_grid* functions, which can classify and draw samples by attributes. Our previous practice was to group the data by myself and then draw the graphs separately. But `ggplot2` provides an easy way to facet directly. we can directly group and draw by attributes. It is the same type of graph, separate the data by attributes, and draw multiple graphs;

Third, for *fill* and *color* parameters，the difference between two parameters is easy to confuse, check in general fill changes the color within shapes, and color changes the outline.

Last but not least, *Geom_bar*. For example, I have aggregated data female 6, male 4, so write the code like this:

`ggplot(data, aes(x = gender, y = value)) + geom_bar()`.

This will prompt "stat_count() can only have an x or y aesthetic". When we change to `ggplot(data, aes(x = gender)) + geom_bar())`, we find that the graph is two bars of the same height, which does not reflect the difference in value. The correct way to write this data is:

`ggplot(data, aes(x = gender, y = value)) + geom_bar(stat = "identity")`.

This is because geom_bar handles data such as female, female, female, ..., male, male, male, male by default, and it will do count, which is the default stat = "count". For details, you can refer to this [stackoverflow page](https://stackoverflow.com/questions/59008974/why-is-stat-identity-necessary-in-geom-bar-in-ggplot). The `geom_bar(stat = "identity")` is also equivalent to `geom_col()`.

Since `ggplot2` and `tidyverse` has various users, R studio created [official cheat sheet](https://www.rstudio.com/resources/cheatsheets/) for everyone to download. It can quickly grasp the main knowledge of ggplot2. You can learn its syntax, code by selecting the part you want.

Part 3 Checklist

This is the check list that we think we must to do when working on a project:

<input type="checkbox" unchecked> Discuss with manager or professor for precise requirements </input>

<input type="checkbox" unchecked> Get description document of dataset </input>

<input type="checkbox" unchecked> Check and deal with the abnormal data (e.g. cleaning data) before visualization</input>

<input type="checkbox" unchecked> Debug</input>

<input type="checkbox" unchecked> Format of the result graph and check details, e.g: data arrangement logic, color, picture copywriting </input>

Part 4 Helpful Content and Links

Aside our knowledge and textbooks, if we find bugs or getting trouble, we can find solutions from the Internet or books. We already mention some in previous content, and here are the list of R coding.

For beginner of R and Rstudio, in order to get familiar with R and `ggplot2`, there are book source we used as extra textbook:

*Discovering Statistics using R* Andy Field, Jeremy Miles and Zoe Field , 2012. and,

*An Introduction to R* W.N.Venables, D.M.Smith and the R core team..

They provide brief description of how to operate R and R Studio and mention visualization inside that good for future advanced R coding.

For helpful links, we have several recommend that we used to find help before:

[Professor Joyce Robbins's website for STAT 5293](https://edav.info)

[tidyverse cookbook](https://rstudio-education.github.io/tidyverse-cookbook/)

[R graphics cookbook](http://www.cookbook-r.com)

In addition, this is one of our notes from last semesters class, since it helpful for us, we put the link here:

[Jiayuan's casual note from STAT5206](https://unruly-age-ae3.notion.site/STAT5206-R-e9a9b224c01c4097b0b09827a2c676aa)
