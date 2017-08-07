# shiny-study

## Part 1 How to build a shiny app

### App template
#### The shortest viable shiny app

``` R
library(shiny)
ui <- fluidPage()
server <- function(input, output){}
shinyApp(ui=ui, server=server)

```

### inputs and outputs

``` R
ui <- fluidPage(
   # *Input() functions,
   # *Output() functions
)
```

#### Inputs

``` R
sliderInput(inputId="num",
  label = "Choose a number",
  value = 25, min = 1, max = 100)
```

there are several input functions
![image](https://github.com/lykkkk/shiny-study/raw/master/SS-1.png)


#### Outputs
![image](https://github.com/lykkkk/shiny-study/raw/master/SS-2.png)

### Server
##### Tell the server how to assemble inputs into outputs
#### 3 rules
1. Save objects to display to output$
```
output$hist
plotOutput("hist")
```

2. Build objects to display with `render*()`
```
server <- function(input, output) {
  output$hist <- renderPlot({
  title <- "100 random normal values"
  hist(rnorm(100),main = title)
  })
}
`render*()` function will create the type of output you wish to make
![image](https://github.com/lykkkk/shiny-study/raw/master/SS-3.png)

3. Access `input` values with `input$`
```R
serevr <- function(input, output) {
  output$hist <- renderPlot({
    hist(rnorm(input$num))
  })
}
```

## Part 2 How to customize reactions

```R
library(shiny)
ui <- fluidPage(
  sliderInput(inputId = "num",
    label = "Choose a number",
    value = 25, min = 1, max = 100),
  plotOutput("hist")
)

server <- function(input, output) {
  output$hist <- renderPlot({
    hist(rnorm(input$num))
  })
}

shinyApp(ui = ui, server = server)
```

### What is Reactivity?

`input$x` change, then `output$y` change
![image](https://github.com/lykkkk/shiny-study/raw/master/SS-4.png)

#### Begin with reactive values
`render*()` builds reactive output to display in UI
When notified that it is invalid, the object created by a `render*()` function will rerun the entire block of code associated with it
![image](https://github.com/lykkkk/shiny-study/raw/master/SS-5.png)
![image](https://github.com/lykkkk/shiny-study/raw/master/SS-6.png)

1. `render*()` functions make objects to display<br>
2. Always save the result to `output$` <br>
3. `render*()` makes an observer object that has a block of code associated with it<br>
4. The object will rerun the entire code block to update itself whenever it is invalidated.

#### Modularize code with reactive()
1. You call a reactive expression like a function
2. Reactive expressions cache their values (the expression will return its most recent value, unless it has become invalidated)

#### Prevent reactions with isolate()



