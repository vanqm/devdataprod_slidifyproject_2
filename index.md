---
title       : Slidify Application about the length of odontoblasts (teeth)
subtitle    : The length of teeth vs Vitamin C (supplement + dose)
author      : Van Mai
job         : Data Scientist
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Application Description

1. Analyze the ToothGrowth data in the R datasets package.
2. The response is the length of odontoblasts (teeth) in each of 10 guinea pigs at each of three dose levels of Vitamin C (0.5, 1, and 2 mg) with each of two delivery methods (orange juice or ascorbic acid).

--- .class #id 

## ui.R

```
library(shiny)
library(jsonlite)

shinyUI(
    pageWithSidebar(
        headerPanel("The length of odontoblasts (teeth)"),
        sidebarPanel(
            h5('The response is the length of odontoblasts (teeth) in each of 10 guinea pigs at each of three dose levels of
Vitamin C (0.5, 1, and 2 mg) with each of two delivery methods (orange juice or ascorbic acid).'),
            radioButtons("dose", 'Levels of Vitamin C', 
                         c("0.5 mg" = "0.5",
                         "1.0 mg" = "1",
                         "2.0 mg" = "2")),
            radioButtons("supp", 'Method', 
                         c("Orange Juice"                  = "OJ",
                           "Ascorbic Acid"                 = "VC",
                           "Orange Juice vs Ascorbic Acid" = "OJ_VC"))
            ),
        mainPanel(  plotOutput('newHist')   )
        )
    )
```

--- .class #id 

## server.R

```
library(shiny)
library(jsonlite)
data(ToothGrowth)

shinyServer(
  function(input, output){
    output$newHist <- renderPlot({
      dose <- input$dose; data <- ToothGrowth[ToothGrowth$dose == dose, ]
      supp <- input$supp; numPlot <- 1
      if (supp == 'OJ'){  data <- data[data$supp == 'OJ', ]   } 
      else if(supp == 'VC') { data <- data[data$supp == 'VC', ]   } 
      else {
        numPlot <- 2
        dataOJ <- data[data$supp == 'OJ', ]
        dataVC <- data[data$supp == 'VC', ]
      }
      par(mfrow = c(2, numPlot))
      if(numPlot == 1) {
        barplot(data$len, ylab = 'Length of teeth', ylim = c(0, 30), col = c(1:10), main = "Length of teeth vs Supp")
      } else {
        barplot(dataOJ$len, ylab = 'Length of teeth', ylim = c(0, 30), col = c(1:10), main = "Length of teeth vs Orange Juice")
        barplot(dataVC$len, ylab = 'Length of teeth', ylim = c(0, 30), col = c(1:10), main = "Length of teeth vs Ascorbic Acid")
      }   
    })
  })
```

--- .class #id 

## A Result

Shiny app is published on [https://vanqm.shinyapps.io/course_project](https://vanqm.shinyapps.io/course_project)

![The example of shiny app](assets/img/shinyapp_example.png)


