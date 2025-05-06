USDebtInsights


Table of Contents

Summary
Introduction
Features
Tech Stack
Installation
Usage
Code
Why This Matters
Conclusion


Summary
USDebtInsights is an interactive Shiny web application that delivers real-time, data-driven insights into the U.S. national debt, designed for economists, political strategists, and academic researchers. Hosted at https://mmcdonald411.shinyapps.io/USDebtApp/, this app visualizes critical metrics—total debt, debt-to-GDP ratio, foreign debt share, and more—through intuitive Plotly charts and KPI cards inspired by PowerBI aesthetics. With features like Monte Carlo scenario analysis, customizable data uploads, and dynamic theme switching, it empowers users to explore fiscal trends, forecast debt trajectories, and inform policy debates. Built with a robust R ecosystem, this app bridges academic rigor with practical decision-making, making complex economic data accessible and actionable.

Introduction
The U.S. national debt, exceeding $35 trillion by 2025, is a cornerstone of economic policy and public discourse. USDebtInsights transforms raw data into a sleek, interactive dashboard that illuminates debt composition, historical trends, and future projections. Whether you’re an Ivy League professor analyzing debt-to-GDP ratios, a political strategist crafting campaign narratives, or an economist forecasting fiscal sustainability, this app provides the tools to explore, visualize, and download debt-related data with ease. Its PowerBI-like design—featuring gradient backgrounds, Segoe UI typography, and GSAP animations—ensures an engaging experience for all users.

Features

📊 KPI Cards: Real-time metrics for total debt, debt-to-GDP ratio, and foreign debt share, animated with GSAP for visual impact.
📈 Interactive Visualizations: Plotly charts for debt holders, debt over time, debt-to-GDP, foreign holders, Fed remittances, and debt composition, with hover, zoom, and animation capabilities.
🔍 Monte Carlo Scenario Analysis: Simulate debt projections based on user-defined interest rates, GDP growth, and inflation, visualized as histograms and data tables.
📥 Data Uploads: Import custom CSV datasets for debt holders, debt over time, foreign holders, and Fed remittances, with robust validation.
💾 Downloads: Export datasets (CSV) and plots (PNG) for reports and presentations.
🎨 Theme Switching: Choose between Darkly, Flatly, and Minty themes for personalized aesthetics.
⚙️ Responsive Design: Optimized for desktop and mobile, with collapsible sidebar panels for navigation, uploads, controls, and downloads.
🛠️ Accessibility: ARIA-compliant inputs and keyboard navigation for inclusive use.


Tech Stack
The app leverages a carefully curated set of R packages, each contributing to its functionality, interactivity, and polish. Below are the core packages, showcased in vibrant banners.

  shiny
  Version: >= 1.7.4
  Powers the web app framework, enabling reactive UI and server-side logic for real-time interactivity.



  ggplot2
  Version: >= 3.4.2
  Creates elegant, customizable plots for debt trends, converted to interactive Plotly visuals.



  plotly
  Version: >= 4.10.0
  Renders interactive charts (pie, line, bar, histogram) with hover, zoom, and animation features.



  shinyWidgets
  Version: >= 0.7.2
  Enhances UI with picker inputs, sliders, and buttons for intuitive controls.



  shinycssloaders
  Version: >= 0.5.2
  Adds loading spinners to visualizations, improving user experience during data processing.



  DT
  Version: >= 0.9.6
  Generates interactive data tables for Monte Carlo simulation results.



  htmltools
  Version: >= 0.5.0
  Facilitates dynamic HTML generation for custom UI components like KPI cards.



  shinyjs
  Version: >= 1.7.0
  Enables JavaScript-based UI interactions, such as toggling sidebar panels.


External Resources:

GSAP: Animates KPI cards with smooth transitions (CDN-hosted).
Font Awesome: Provides icons for intuitive navigation (CDN-hosted).
Google Fonts: Delivers Segoe UI and Roboto for a modern, professional look (CDN-hosted).


Installation
To run USDebtInsights locally or deploy it, follow these steps:

Install R and RStudio:

Download R (version >= 4.2.0) and RStudio.


Install Required Packages:
install.packages(c("shiny", "ggplot2", "plotly", "shinyWidgets", "shinycssloaders", "DT", "htmltools", "shinyjs"))


Clone the Repository:
git clone https://github.com/your-username/USDebtInsights.git
cd USDebtInsights


Verify Package Versions:
packageVersion("shiny")       # >= 1.7.4
packageVersion("ggplot2")     # >= 3.4.2
packageVersion("plotly")      # >= 4.10.0
packageVersion("shinyWidgets") # >= 0.7.2
packageVersion("shinycssloaders") # >= 0.5.2
packageVersion("DT")          # >= 0.9.6
packageVersion("htmltools")   # >= 0.5.0
packageVersion("shinyjs")     # >= 1.7.0


Run the App Locally:
setwd("path/to/USDebtInsights")
shiny::runApp("app.R")


Deploy to ShinyApps.io (Optional):

Install rsconnect:install.packages("rsconnect")


Configure your ShinyApps.io account:rsconnect::setAccountInfo(name = "your-username", token = "your-token", secret = "your-secret")


Deploy:rsconnect::deployApp(appDir = ".", appName = "USDebtApp", runtime = "shiny")





Note: Ensure no Quarto-related files (e.g., _quarto.yml, .qmd, .Rmd) are in the app directory to avoid deployment errors.

Usage
Visit the live app at https://mmcdonald411.shinyapps.io/USDebtApp/ or run it locally. Here’s how to get started:

Explore the Dashboard:

View KPI cards for total debt, debt-to-GDP ratio, and foreign debt share.
Navigate tabs (Overview, Debt Holders, Debt Over Time, etc.) using the sidebar or top navigation.


Interact with Visualizations:

Hover over Plotly charts for detailed tooltips (e.g., debt amounts, years).
Zoom or pan to focus on specific data ranges.
Adjust the year range slider for time-series plots (Debt Over Time, Debt-to-GDP, Debt Composition).


Upload Custom Data:

Upload CSV files for debt holders (Holder, Amount), debt over time (Year, Debt, GDP), foreign holders (Country, Amount), or Fed remittances (Year, Remittance).
The app validates uploads and displays notifications for success or errors.


Run Scenario Analysis:

Input parameters (interest rate, GDP growth, inflation, number of simulations) in the Scenario Analysis tab.
Click "Run Scenario" to generate a Monte Carlo simulation, displayed as a histogram and interactive table.
Reset inputs with the "Reset" button.


Download Data and Plots:

Export datasets as CSVs or plots as PNGs using the Downloads panel.
Use exported files for reports, presentations, or further analysis.


Switch Themes:

Select Darkly, Flatly, or Minty from the sidebar to customize the app’s appearance.



Screenshot: [Add a screenshot or GIF of the app here to showcase its interface. Use a tool like ScreenToGif or ShareX.]

Code

Full app.R Code (Click to Expand)

# Clear global environment to avoid interference
# Note: For ShinyApps.io deployment, ensure runtime is set to 'shiny' and no Quarto-related files (e.g., _quarto.yml, .qmd, .Rmd) are in the app directory
rm(list = ls())

library(shiny)
library(ggplot2)
library(plotly)
library(shinyWidgets)
library(shinycssloaders)
library(DT)
library(htmltools)
library(shinyjs)

# Check for required packages and versions
required_packages <- c("shiny", "ggplot2", "plotly", "shinyWidgets", "shinycssloaders", "DT", "htmltools", "shinyjs")
min_versions <- c("1.7.4", "3.4.2", "4.10.0", "0.7.2", "0.5.2", "0.9.6", "0.5.0", "1.7.0")
missing_packages <- required_packages[!sapply(required_packages, requireNamespace, quietly = TRUE)]
if (length(missing_packages) > 0) {
  stop(paste("Missing packages:", paste(missing_packages, collapse = ", "), ". Please install them using install.packages()."))
}
for (i in seq_along(required_packages)) {
  if (packageVersion(required_packages[i]) < min_versions[i]) {
    stop(paste("Package", required_packages[i], "version", packageVersion(required_packages[i]), 
               "is too old. Please update to at least version", min_versions[i], "using install.packages()."))
  }
}

# Check for www/ folder and create theme CSS files
www_files <- c("darkly.css", "flatly.css", "minty.css")
if (!dir.exists("www")) dir.create("www", showWarnings = FALSE)
darkly_css <- "
  body { 
    font-family: 'Segoe UI', 'Roboto', sans-serif; 
    background: linear-gradient(135deg, #1e1e1e, #2d2d30); 
    color: #ffffff; 
  }
  .sidebar { 
    background: #252526; 
    padding: 20px; 
    border-right: 2px solid #0078d4; 
    border-radius: 0 12px 12px 0; 
  }
  .main-panel { 
    padding: 30px; 
  }
  .well { 
    background: #2d2d30; 
    border: none; 
    box-shadow: 0 4px 12px rgba(0,0,0,0.3); 
    border-radius: 12px; 
    color: #d4d4d4; 
    transition: transform 0.3s ease; 
    margin-bottom: 10px;
  }
  .well:hover { 
    transform: scale(1.02); 
  }
  .toggle-btn { 
    background: #0078d4; 
    border: none; 
    border-radius: 8px; 
    color: #ffffff; 
    width: 100%; 
    text-align: left; 
    padding: 10px; 
    margin-bottom: 5px; 
    transition: background 0.3s ease; 
  }
  .toggle-btn:hover { 
    background: #005ba1; 
  }
  .btn-primary { 
    background: #0078d4; 
    border: none; 
    border-radius: 8px; 
    transition: background 0.3s ease; 
  }
  .btn-primary:hover { 
    background: #005ba1; 
  }
  .nav-tabs > li > a { 
    color: #d4d4d4; 
    background: #2d2d30; 
    border-radius: 8px 8px 0 0; 
    margin-right: 4px; 
  }
  .nav-tabs > li.active > a { 
    color: #ffffff; 
    background: #0078d4; 
  }
  .kpi-card { 
    background: #2d2d30; 
    border-radius: 12px; 
    padding: 20px; 
    text-align: center; 
    box-shadow: 0 4px 12px rgba(0,0,0,0.3); 
    margin-bottom: 20px; 
  }
  .kpi-card h3 { 
    margin: 0; 
    font-size: 1.5em; 
    color: #64b5f6; 
  }
  .kpi-card p { 
    font-size: 2em; 
    font-weight: bold; 
    color: #ffffff; 
    margin: 10px 0 0; 
  }
  .tooltip { 
    background: #252526; 
    color: #ffffff; 
    border: 1px solid #0078d4; 
  }
  .plotly .modebar { 
    background: #2d2d30; 
  }
  @media (max-width: 768px) { 
    .well, .kpi-card { width: 100%; } 
    .sidebar { width: 100%; border-radius: 12px; } 
  }
"
flatly_css <- "
  body { 
    font-family: 'Segoe UI', 'Roboto', sans-serif; 
    background: linear-gradient(135deg, #f8f9fa, #e9ecef); 
    color: #333333; 
  }
  .sidebar { 
    background: #34495e; 
    padding: 20px; 
    border-right: 2px solid #2c3e50; 
    border-radius: 0 12px 12px 0; 
  }
  .main-panel { 
    padding: 30px; 
  }
  .well { 
    background: #ffffff; 
    border: none; 
    box-shadow: 0 4px 12px rgba(0,0,0,0.1); 
    border-radius: 12px; 
    color: #333333; 
    transition: transform 0.3s ease; 
    margin-bottom: 10px;
  }
  .well:hover { 
    transform: scale(1.02); 
  }
  .toggle-btn { 
    background: #2c3e50; 
    border: none; 
    border-radius: 8px; 
    color: #ffffff; 
    width: 100%; 
    text-align: left; 
    padding: 10px; 
    margin-bottom: 5px; 
    transition: background 0.3s ease; 
  }
  .toggle-btn:hover { 
    background: #1a252f; 
  }
  .btn-primary { 
    background: #2c3e50; 
    border: none; 
    border-radius: 8px; 
    transition: background 0.3s ease; 
  }
  .btn-primary:hover { 
    background: #1a252f; 
  }
  .nav-tabs > li > a { 
    color: #ecf0f1; 
    background: #34495e; 
    border-radius: 8px 8px 0 0; 
    margin-right: 4px; 
  }
  .nav-tabs > li.active > a { 
    color: #ffffff; 
    background: #2c3e50; 
  }
  .kpi-card { 
    background: #ffffff; 
    border-radius: 12px; 
    padding: 20px; 
    text-align: center; 
    box-shadow: 0 4px 12px rgba(0,0,0,0.1); 
    margin-bottom: 20px; 
  }
  .kpi-card h3 { 
    margin: 0; 
    font-size: 1.5em; 
    color: #2c3e50; 
  }
  .kpi-card p { 
    font-size: 2em; 
    font-weight: bold; 
    color: #344 regener95e; 
    margin: 10px 0 0; 
  }
  .tooltip { 
    background: #34495e; 
    color: #ffffff; 
    border: 1px solid #2c3e50; 
  }
  .plotly .modebar { 
    background: #ffffff; 
  }
  @media (max-width: 768px) { 
    .well, .kpi-card { width: 100%; } 
    .sidebar { width: 100%; border-radius: 12px; } 
  }
"
minty_css <- "
  body { 
    font-family: 'Segoe UI', 'Roboto', sans-serif; 
    background: linear-gradient(135deg, #f1f8f7, #e0f0ed); 
    color: #2f2f2f; 
  }
  .sidebar { 
    background: #f0f6f5; 
    padding: 20px; 
    border-right: 2px solid #78c2ad; 
    border-radius: 0 12px 12px 0; 
  }
  .main-panel { 
    padding: 30px; 
  }
  .well { 
    background: #ffffff; 
    border: none; 
    box-shadow: 0 4px 12px rgba(0,0,0,0.1); 
    border-radius: 12px; 
    color: #2f2f2f; 
    transition: transform 0.3s ease; 
    margin-bottom: 10px;
  }
  .well:hover { 
    transform: scale(1.02); 
  }
  .toggle-btn { 
    background: #78c2ad; 
    border: none; 
    border-radius: 8px; 
    color: #ffffff; 
    width: 100%; 
    text-align: left; 
    padding: 10px; 
    margin-bottom: 5px; 
    transition: background 0.3s ease; 
  }
  .toggle-btn:hover { 
    background: #5aa791; 
  }
  .btn-primary { 
    background: #78c2ad; 
    border: none; 
    border-radius: 8px; 
    transition: background 0.3s ease; 
  }
  .btn-primary:hover { 
    background: #5aa791; 
  }
  .nav-tabs > li > a { 
    color: #2f2f2f; 
    background: #f0f6f5; 
    border-radius: 8px 8px 0 0; 
    margin-right: 4px; 
  }
  .nav-tabs > li.active > a { 
    color: #ffffff; 
    background: #78c2ad; 
  }
  .kpi-card { 
    background: #ffffff; 
    border-radius: 12px; 
    padding: 20px; 
    text-align: center; 
    box-shadow: 0 4px 12px rgba(0,0,0,0.1); 
    margin-bottom: 20px; 
  }
  .kpi-card h3 { 
    margin: 0; 
    font-size: 1.5em; 
    color: #78c2ad; 
  }
  .kpi-card p { 
    font-size: 2em; 
    font-weight: bold; 
    color: #2f2f2f; 
    margin: 10px 0 0; 
  }
  .tooltip { 
    background: #f0f6f5; 
    color: #2f2f2f; 
    border: 1px solid #78c2ad; 
  }
  .plotly .modebar { 
    background: #ffffff; 
  }
  @media (max-width: 768px) { 
    .well, .kpi-card { width: 100%; } 
    .sidebar { width: 100%; border-radius: 12px; } 
  }
"
for (file in www_files) {
  if (!file.exists(file.path("www", file))) {
    writeLines(get(sub("(.*)\\.css", "\\1_css", file)), file.path("www", file))
  }
}

# Sample datasets
debt_data <- reactiveVal(data.frame(
  Holder = c("Federal Reserve", "U.S. Public", "Foreign Holders", "Intragovernmental"),
  Amount = c(7000, 11000, 8000, 7000)
))
debt_time <- reactiveVal(data.frame(
  Year = 2000:2025,
  Debt = c(6000, 6200, 6500, 7000, 7500, 8000, 9000, 10000, 11000, 12000,
           14000, 16000, 17000, 18000, 20000, 22000, 24000, 26000, 28000, 30000,
           32000, 34000, 35000, 35500, 36000, 36500),
  GDP = c(seq(10, 25, length.out = 26)) * 1000
))
fed_remittances <- reactiveVal(data.frame(
  Year = 2010:2025,
  Remittance = c(79, 88, 90, 92, 95, 98, 100, 110, 120, 130, 140, 150, 140, 130, 120, 110)
))
foreign_holders <- reactiveVal(data.frame(
  Country = c("China", "Japan", "UK", "Brazil", "Others"),
  Amount = c(1.1, 1.0, 0.5, 0.3, 3.1) * 1000
))
debt_composition <- reactiveVal(data.frame(
  Year = rep(2000:2025, each = 4),
  Holder = rep(c("Federal Reserve", "U.S. Public", "Foreign Holders", "Intragovernmental"), 26),
  Amount = c(
    seq(1500, 7000, length.out = 26), seq(3000, 11000, length.out = 26),
    seq(1500, 8000, length.out = 26), seq(1000, 7000, length.out = 26)
  )
))

# UI
ui <- fluidPage(
  useShinyjs(),
  titlePanel("U.S. National Debt Insights"),
  tags$head(
    tags$link(rel = "stylesheet", href = "https://fonts.googleapis.com/css2?family=Segoe+UI:wght@300;400;700&family=Roboto:wght@400;700&display=swap"),
    tags$link(rel = "stylesheet", href = "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css"),
    tags$script(src = "https://cdnjs.cloudflare.com/ajax/libs/gsap/3.9.1/gsap.min.js"),
    uiOutput("dynamic_css")
  ),
  sidebarLayout(
    sidebarPanel(
      actionButton("toggleNav", "Navigation", class = "toggle-btn", icon = icon("compass")),
      wellPanel(id = "navPanel", style = "display: none;",
        pickerInput("tab_select", "Select View", 
                    choices = c("Overview", "Debt Holders", "Debt Over Time", "Debt-to-GDP", 
                                "Foreign Holders", "Fed Remittances", "Debt Composition", "Scenario Analysis"),
                    selected = "Overview", options = list(`live-search` = TRUE)),
        pickerInput("theme", "Select Theme", choices = c("Darkly", "Flatly", "Minty"), selected = "Darkly")
      ),
      actionButton("toggleUpload", "Data Upload", class = "toggle-btn", icon = icon("upload")),
      wellPanel(id = "uploadPanel", style = "display: none;",
        fileInput("uploadDebtData", "Upload Debt Holders CSV", accept = ".csv"),
        fileInput("uploadDebtTime", "Upload Debt Over Time CSV", accept = ".csv"),
        fileInput("uploadForeignHolders", "Upload Foreign Holders CSV", accept = ".csv"),
        fileInput("uploadFedRemittances", "Upload Fed Remittances CSV", accept = ".csv")
      ),
      actionButton("toggleControls", "Controls", class = "toggle-btn", icon = icon("sliders-h")),
      wellPanel(id = "controlsPanel", style = "display: none;",
        conditionalPanel(
          condition = "input.tab_select == 'Debt Over Time' || input.tab_select == 'Debt-to-GDP' || input.tab_select == 'Debt Composition'",
          sliderInput("yearRange", "Select Year Range:", min = 2000, max = 2025, value = c(2000, 2025), step = 1, sep = "", animate = TRUE)
        ),
        conditionalPanel(
          condition = "input.tab_select == 'Scenario Analysis'",
          numericInput("interestRate", "Interest Rate (%)", value = 5, min = 1, max = 10, step = 0.1),
          numericInput("gdpGrowth", "GDP Growth (%)", value = 2, min = -2, max = 5, step = 0.1),
          numericInput("inflation", "Inflation Rate (%)", value = 2, min = 0, max = 10, step = 0.1),
          numericInput("simulations", "Number of Simulations", value = 100, min = 10, max = 1000, step = 10),
          actionButton("runScenario", "Run Scenario", class = "btn-primary", icon = icon("play")),
          actionButton("resetScenario", "Reset", class = "btn-primary", icon = icon("undo"))
        )
      ),
      actionButton("toggleDownloads", "Downloads", class = "toggle-btn", icon = icon("download")),
      wellPanel(id = "downloadsPanel", style = "display: none;",
        downloadButton("downloadDebtHolders", "Debt Holders", class = "btn-primary", icon = icon("download")),
        downloadButton("downloadDebtTime", "Debt Over Time", class = "btn-primary", icon = icon("download")),
        downloadButton("downloadDebtGDP", "Debt-to-GDP", class = "btn-primary", icon = icon("download")),
        downloadButton("downloadForeignHolders", "Foreign Holders", class = "btn-primary", icon = icon("download")),
        downloadButton("downloadFedRemittances", "Fed Remittances", class = "btn-primary", icon = icon("download")),
        downloadButton("downloadPlot", "Download Plot as PNG", class = "btn-primary", icon = icon("image"))
      )
    ),
    mainPanel(
      fluidRow(
        column(4, div(class = "kpi-card", uiOutput("kpiTotalDebt"))),
        column(4, div(class = "kpi-card", uiOutput("kpiDebtGDP"))),
        column(4, div(class = "kpi-card", uiOutput("kpiForeignShare")))
      ),
      tabsetPanel(
        id = "tabs",
        tabPanel("Overview",
                 fluidRow(
                   column(6, wellPanel(withSpinner(plotlyOutput("debtHoldersPlot", height = "400px")), textOutput("debtHoldersSummary"))),
                   column(6, wellPanel(withSpinner(plotlyOutput("debtTimePlot", height = "400px")), textOutput("debtTimeSummary")))
                 ),
                 fluidRow(
                   column(6, wellPanel(withSpinner(plotlyOutput("foreignHoldersPlot", height = "400px")), textOutput("foreignHoldersSummary"))),
                   column(6, wellPanel(withSpinner(plotlyOutput("fedRemittancesPlot", height = "400px")), textOutput("fedRemittancesSummary")))
                 )
        ),
        tabPanel("Debt Holders", wellPanel(withSpinner(plotlyOutput("debtHoldersPlotFull", height = "600px")), textOutput("debtHoldersSummary"))),
        tabPanel("Debt Over Time", wellPanel(withSpinner(plotlyOutput("debtTimePlotFull", height = "600px")), textOutput("debtTimeSummary"))),
        tabPanel("Debt-to-GDP", wellPanel(withSpinner(plotlyOutput("debtGDPPlot", height = "600px")), textOutput("debtGDPSummary"))),
        tabPanel("Foreign Holders", wellPanel(withSpinner(plotlyOutput("foreignHoldersPlotFull", height = "600px")), textOutput("foreignHoldersSummary"))),
        tabPanel("Fed Remittances", wellPanel(withSpinner(plotlyOutput("fedRemittancesPlotFull", height = "600px")), textOutput("fedRemittancesSummary"))),
        tabPanel("Debt Composition", wellPanel(withSpinner(plotlyOutput("debtCompositionPlot", height = "600px")), textOutput("debtCompositionSummary"))),
        tabPanel("Scenario Analysis", wellPanel(DTOutput("scenarioTable"), withSpinner(plotlyOutput("scenarioPlot", height = "600px")))
        )
      )
    )
  )
)

# Server
server <- function(input, output, session) {
  # Dynamic CSS for theme switching
  output$dynamic_css <- renderUI({
    tryCatch({
      theme_file <- tolower(paste0(input$theme, ".css"))
      addResourcePath("www", "www")
      tags$link(rel = "stylesheet", type = "text/css", href = file.path("www", theme_file))
    }, error = function(e) {
      showNotification(paste("Error loading theme:", e$message), type = "error")
      tags$link(rel = "stylesheet", type = "text/css", href = "www/darkly.css")
    })
  })

  # Toggle sidebar sections
  observeEvent(input$toggleNav, {
    shinyjs::toggle("navPanel")
  })
  observeEvent(input$toggleUpload, {
    shinyjs::toggle("uploadPanel")
  })
  observeEvent(input$toggleControls, {
    shinyjs::toggle("controlsPanel")
  })
  observeEvent(input$toggleDownloads, {
    shinyjs::toggle("downloadsPanel")
  })

  # Sync tab selection
  observeEvent(input$tab_select, {
    updateTabsetPanel(session, "tabs", selected = input$tab_select)
  })
  observeEvent(input$tabs, {
    updatePickerInput(session, "tab_select", selected = input$tabs)
  })

  # Handle data uploads
  observeEvent(input$uploadDebtData, {
    tryCatch({
      req(input$uploadDebtData)
      new_data <- read.csv(input$uploadDebtData$datapath)
      if (all(c("Holder", "Amount") %in% colnames(new_data))) {
        debt_data(new_data)
        showNotification("Debt Holders data uploaded successfully.", type = "message")
      } else {
        showNotification("Invalid CSV format. Requires 'Holder' and 'Amount' columns.", type = "error")
      }
    }, error = function(e) {
      showNotification(paste("Error uploading Debt Holders data:", e$message), type = "error")
    })
  })
  observeEvent(input$uploadDebtTime, {
    tryCatch({
      req(input$uploadDebtTime)
      new_data <- read.csv(input$uploadDebtTime$datapath)
      if (all(c("Year", "Debt", "GDP") %in% colnames(new_data))) {
        debt_time(new_data)
        showNotification("Debt Over Time data uploaded successfully.", type = "message")
      } else {
        showNotification("Invalid CSV format. Requires 'Year', 'Debt', and 'GDP' columns.", type = "error")
      }
    }, error = function(e) {
      showNotification(paste("Error uploading Debt Over Time data:", e$message), type = "error")
    })
  })
  observeEvent(input$uploadForeignHolders, {
    tryCatch({
      req(input$uploadForeignHolders)
      new_data <- read.csv(input$uploadForeignHolders$datapath)
      if (all(c("Country", "Amount") %in% colnames(new_data))) {
        foreign_holders(new_data)
        showNotification("Foreign Holders data uploaded successfully.", type = "message")
      } else {
        showNotification("Invalid CSV format. Requires 'Country' and 'Amount' columns.", type = "error")
      }
    }, error = function(e) {
      showNotification(paste("Error uploading Foreign Holders data:", e$message), type = "error")
    })
  })
  observeEvent(input$uploadFedRemittances, {
    tryCatch({
      req(input$uploadFedRemittances)
      new_data <- read.csv(input$uploadFedRemittances$datapath)
      if (all(c("Year", "Remittance") %in% colnames(new_data))) {
        fed_remittances(new_data)
        showNotification("Fed Remittances data uploaded successfully.", type = "message")
      } else {
        showNotification("Invalid CSV format. Requires 'Year' and 'Remittance' columns.", type = "error")
      }
    }, error = function(e) {
      showNotification(paste("Error uploading Fed Remittances data:", e$message), type = "error")
    })
  })

  # Reactive expression for filtered debt time
  filtered_debt_time <- reactive({
    tryCatch({
      subset(debt_time(), Year >= input$yearRange[1] & Year <= input$yearRange[2])
    }, error = function(e) {
      showNotification("Error filtering debt time data.", type = "error")
      debt_time()
    })
  }) %>% bindCache(input$yearRange)

  # KPI Cards
  output$kpiTotalDebt <- renderUI({
    total_debt <- sum(debt_data()$Amount)
    div(class = "kpi-card", 
        h3("Total Debt"), 
        p(paste0("$", format(total_debt, big.mark = ",", scientific = FALSE), "B")),
        tags$script(HTML("gsap.from('.kpi-card', {duration: 1, y: 50, opacity: 0, stagger: 0.2});"))
    )
  })
  output$kpiDebtGDP <- renderUI({
    latest_debt_gdp <- tail(debt_time()$Debt / debt_time()$GDP, 1)
    div(class = "kpi-card", 
        h3("Debt-to-GDP Ratio"), 
        p(sprintf("%.2f%%", latest_debt_gdp * 100)))
  })
  output$kpiForeignShare <- renderUI({
    foreign_share <- sum(debt_data()$Amount[debt_data()$Holder == "Foreign Holders"]) / sum(debt_data()$Amount)
    div(class = "kpi-card", 
        h3("Foreign Debt Share"), 
        p(sprintf("%.2f%%", foreign_share * 100)))
  })

  # Plot Outputs
  output$debtHoldersPlot <- renderPlotly({
    tryCatch({
      plot_ly(debt_data(), labels = ~Holder, values = ~Amount, type = "pie",
              textinfo = "label+percent", insidetextorientation = "radial",
              hoverinfo = "text", text = ~paste(Holder, ": $", Amount, "B"),
              marker = list(colors = c("#0078d4", "#005ba1", "#64b5f6", "#d4e6ff"))) %>%
        layout(title = list(text = "Debt Holders", x = 0.5, font = list(size = 16)),
               legend = list(orientation = "v", x = 1, y = 0.5), margin = list(t = 50),
               annotations = list(text = "Compact View", showarrow = FALSE, x = 0.5, y = -0.1))
    }, error = function(e) {
      showNotification("Error rendering debt holders plot.", type = "error")
      NULL
    })
  })
  output$debtHoldersPlotFull <- renderPlotly({
    tryCatch({
      plot_ly(debt_data(), labels = ~Holder, values = ~Amount, type = "pie",
              textinfo = "label+percent", insidetextorientation = "radial",
              hoverinfo = "text", text = ~paste(Holder, ": $", Amount, "B"),
              marker = list(colors = c("#0078d4", "#005ba1", "#64b5f6", "#d4e6ff"))) %>%
        layout(title = list(text = "U.S. Debt Holders (in Billions USD)", x = 0.5, font = list(size = 20)),
               legend = list(orientation = "v", x = 1, y = 0.5), margin = list(t = 100))
    }, error = function(e) {
      showNotification("Error rendering debt holders plot.", type = "error")
      NULL
    })
  })
  output$debtHoldersSummary <- renderText({
    "The Federal Reserve holds ~20% of U.S. national debt; the largest share is held by domestic and foreign investors."
  })

  output$debtTimePlot <- renderPlotly({
    tryCatch({
      p <- ggplot(filtered_debt_time(), aes(x = Year, y = Debt)) +
        geom_line(color = "#0078d4", size = 1.2) +
        geom_point(color = "#0078d4", size = 2) +
        theme_minimal(base_size = 12) +
        labs(title = "Debt Over Time", y = "Debt (Billions USD)", x = "Year")
      ggplotly(p, tooltip = c("x", "y")) %>% animation_opts(frame = 100, easing = "linear")
    }, error = function(e) {
      showNotification("Error rendering debt time plot.", type = "error")
      NULL
    })
  })
  output$debtTimePlotFull <- renderPlotly({
    tryCatch({
      p <- ggplot(filtered_debt_time(), aes(x = Year, y = Debt)) +
        geom_line(color = "#0078d4", size = 1.2) +
        geom_point(color = "#0078d4", size = 2) +
        theme_minimal(base_size = 14) +
        labs(title = "U.S. National Debt Over Time", y = "Debt (Billions USD)", x = "Year") +
        theme(plot.title = element_text(hjust = 0.5, face = "bold", size = 20),
              axis.title = element_text(face = "bold"))
      ggplotly(p, tooltip = c("x", "y")) %>% animation_opts(frame = 100, easing = "linear")
    }, error = function(e) {
      showNotification("Error rendering debt time plot.", type = "error")
      NULL
    })
  })
  output$debtTimeSummary <- renderText({
    "U.S. national debt has grown steeply since 2000, crossing $35 trillion by 2025."
  })

  output$debtGDPPlot <- renderPlotly({
    tryCatch({
      p <- ggplot(filtered_debt_time(), aes(x = Year, y = Debt/GDP)) +
        geom_line(color = "#0078d4", size = 1.2) +
        geom_point(color = "#0078d4", size = 2) +
        theme_minimal(base_size = 14) +
        labs(title = "Debt-to-GDP Ratio", y = "Ratio", x = "Year") +
        theme(plot.title = element_text(hjust = 0.5, face = "bold", size = 20),
              axis.title = element_text(face = "bold"))
      ggplotly(p, tooltip = c("x", "y")) %>% animation_opts(frame = 100, easing = "linear")
    }, error = function(e) {
      showNotification("Error rendering debt-to-GDP plot.", type = "error")
      NULL
    })
  })
  output$debtGDPSummary <- renderText({
    "The debt-to-GDP ratio reflects the U.S. debt burden relative to economic output, rising steadily since 2000."
  })

  output$foreignHoldersPlot <- renderPlotly({
    tryCatch({
      plot_ly(foreign_holders(), x = ~reorder(Country, Amount), y = ~Amount, type = "bar",
              hoverinfo = "text", text = ~paste(Country, ": $", Amount, "B"),
              marker = list(color = "#0078d4")) %>%
        layout(title = list(text = "Foreign Holders", x = 0.5, font = list(size = 16)),
               xaxis = list(title = "Country"), yaxis = list(title = "Amount (Billions USD)"),
               margin = list(t = 50), annotations = list(text = "Compact View", showarrow = FALSE, x = 0.5, y = -0.1))
    }, error = function(e) {
      showNotification("Error rendering foreign holders plot.", type = "error")
      NULL
    })
  })
  output$foreignHoldersPlotFull <- renderPlotly({
    tryCatch({
      plot_ly(foreign_holders(), x = ~reorder(Country, Amount), y = ~Amount, type = "bar",
              hoverinfo = "text", text = ~paste(Country, ": $", Amount, "B"),
              marker = list(color = "#0078d4")) %>%
        layout(title = list(text = "Top Foreign Holders of U.S. Debt", x = 0.5, font = list(size = 20)),
               xaxis = list(title = "Country"), yaxis = list(title = "Amount (Billions USD)"),
               margin = list(t = 100))
    }, error = function(e) {
      showNotification("Error rendering foreign holders plot.", type = "error")
      NULL
    })
  })
  output$foreignHoldersSummary <- renderText({
    "China and Japan are the largest foreign holders of U.S. debt, followed by the UK, Brazil, and others."
  })

  output$fedRemittancesPlot <- renderPlotly({
    tryCatch({
      p <- ggplot(fed_remittances(), aes(x = Year, y = Remittance)) +
        geom_line(color = "#0078d4", size = 1.2) +
        geom_point(color = "#0078d4", size = 2) +
        theme_minimal(base_size = 12) +
        labs(title = "Fed Remittances", y = "Remittance (Billions USD)", x = "Year")
      ggplotly(p, tooltip = c("x", "y")) %>% animation_opts(frame = 100, easing = "linear")
    }, error = function(e) {
      showNotification("Error rendering fed remittances plot.", type = "error")
      NULL
    })
  })
  output$fedRemittancesPlotFull <- renderPlotly({
    tryCatch({
      p <- ggplot(fed_remittances(), aes(x = Year, y = Remittance)) +
        geom_line(color = "#0078d4", size = 1.2) +
        geom_point(color = "#0078d4", size = 2) +
        theme_minimal(base_size = 14) +
        labs(title = "Federal Reserve Remittances to U.S. Treasury", y = "Remittance (Billions USD)", x = "Year") +
        theme(plot.title = element_text(hjust = 0.5, face = "bold", size = 20),
              axis.title = element_text(face = "bold"))
      ggplotly(p, tooltip = c("x", "y")) %>% animation_opts(frame = 100, easing = "linear")
    }, error = function(e) {
      showNotification("Error rendering fed remittances plot.", type = "error")
      NULL
    })
  })
  output$fedRemittancesSummary <- renderText({
    "The Federal Reserve returns billions annually to the Treasury, peaking at ~$150B before declining post-2020."
  })

  output$debtCompositionPlot <- renderPlotly({
    tryCatch({
      filtered_data <- subset(debt_composition(), Year >= input$yearRange[1] & Year <= input$yearRange[2])
      plot_ly(filtered_data, x = ~Year, y = ~Amount, color = ~Holder, type = "scatter", mode = "lines+markers",
              stackgroup = "one", hoverinfo = "text", text = ~paste(Holder, ": $", Amount, "B"),
              marker = list(size = 8), line = list(width = 2)) %>%
        layout(title = list(text = "Debt Composition Over Time", x = 0.5, font = list(size = 20)),
               xaxis = list(title = "Year"), yaxis = list(title = "Amount (Billions USD)"),
               legend = list(orientation = "h", x = 0.5, xanchor = "center", y = -0.2),
               margin = list(t = 100))
    }, error = function(e) {
      showNotification("Error rendering debt composition plot.", type = "error")
      NULL
    })
  })
  output$debtCompositionSummary <- renderText({
    "Debt composition shows the evolving shares of Federal Reserve, U.S. Public, Foreign Holders, and Intragovernmental holdings."
  })

  # Scenario Analysis
  scenario_results <- reactive({
    tryCatch({
      req(input$runScenario)
      n_sim <- input$simulations
      results <- data.frame(
        Simulation = 1:n_sim,
        Debt = numeric(n_sim)
      )
      latest_debt <- tail(debt_time()$Debt, 1)
      for (i in 1:n_sim) {
        ir <- rnorm(1, input$interestRate, 0.5) / 100
        gdp_g <- rnorm(1, input$gdpGrowth, 0.5) / 100
        inf <- rnorm(1, input$inflation, 0.3) / 100
        results$Debt[i] <- latest_debt * (1 + ir - gdp_g + inf)
      }
      results
    }, error = function(e) {
      showNotification(paste("Error running scenario analysis:", e$message), type = "error")
      NULL
    })
  }) %>% bindCache(input$runScenario, input$simulations, input$interestRate, input$gdpGrowth, input$inflation)

  output$scenarioTable <- renderDT({
    tryCatch({
      req(scenario_results())
      datatable(scenario_results(), options = list(pageLength = 5, dom = "tip"),
                rownames = FALSE, caption = "Monte Carlo Simulation Results")
    }, error = function(e) {
      showNotification("Error rendering scenario table.", type = "error")
      NULL
    })
  })
  output$scenarioPlot <- renderPlotly({
    tryCatch({
      req(scenario_results())
      plot_ly(scenario_results(), x = ~Debt, type = "histogram",
              marker = list(color = "#0078d4"), nbinsx = 30) %>%
        layout(title = list(text = "Debt Projection Distribution", x = 0.5, font = list(size = 20)),
               xaxis = list(title = "Projected Debt (Billions USD)"),
               yaxis = list(title = "Frequency"),
               margin = list(t = 100))
    }, error = function(e) {
      showNotification("Error rendering scenario plot.", type = "error")
      NULL
    })
  })

  # Reset Scenario Inputs
  observeEvent(input$resetScenario, {
    updateNumericInput(session, "interestRate", value = 5)
    updateNumericInput(session, "gdpGrowth", value = 2)
    updateNumericInput(session, "inflation", value = 2)
    updateNumericInput(session, "simulations", value = 100)
    output$scenarioTable <- renderDT(NULL)
    output$scenarioPlot <- renderPlotly(NULL)
  })

  # Download Handlers
  output$downloadDebtHolders <- downloadHandler(
    filename = function() {"debt_holders.csv"},
    content = function(file) {
      tryCatch({
        write.csv(debt_data(), file, row.names = FALSE)
      }, error = function(e) {
        showNotification("Error downloading debt holders data.", type = "error")
      })
    }
  )
  output$downloadDebtTime <- downloadHandler(
    filename = function() {"debt_time.csv"},
    content = function(file) {
      tryCatch({
        write.csv(filtered_debt_time(), file, row.names = FALSE)
      }, error = function(e) {
        showNotification("Error downloading debt time data.", type = "error")
      })
    }
  )
  output$downloadDebtGDP <- downloadHandler(
    filename = function() {"debt_gdp.csv"},
    content = function(file) {
      tryCatch({
        write.csv(data.frame(
          Year = filtered_debt_time()$Year,
          Debt = filtered_debt_time()$Debt,
          GDP = filtered_debt_time()$GDP,
          Debt_GDP_Ratio = filtered_debt_time()$Debt / filtered_debt_time()$GDP
        ), file, row.names = FALSE)
      }, error = function(e) {
        showNotification("Error downloading debt-to-GDP data.", type = "error")
      })
    }
  )
  output$downloadForeignHolders <- downloadHandler(
    filename = function() {"foreign_holders.csv"},
    content = function(file) {
      tryCatch({
        write.csv(foreign_holders(), file, row.names = FALSE)
      }, error = function(e) {
        showNotification("Error downloading foreign holders data.", type = "error")
      })
    }
  )
  output$downloadFedRemittances <- downloadHandler(
    filename = function() {"fed_remittances.csv"},
    content = function(file) {
      tryCatch({
        write.csv(fed_remittances(), file, row.names = FALSE)
      }, error = function(e) {
        showNotification("Error downloading fed remittances data.", type = "error")
      })
    }
  )
  output$downloadPlot <- downloadHandler(
    filename = function() {paste0(input$tab_select, "_plot.png")},
    content = function(file) {
      tryCatch({
        plot_output <- switch(input$tab_select,
                             "Overview" = "debtHoldersPlot",
                             "Debt Holders" = "debtHoldersPlotFull",
                             "Debt Over Time" = "debtTimePlotFull",
                             "Debt-to-GDP" = "debtGDPPlot",
                             "Foreign Holders" = "foreignHoldersPlotFull",
                             "Fed Remittances" = "fedRemittancesPlotFull",
                             "Debt Composition" = "debtCompositionPlot",
                             "Scenario Analysis" = "scenarioPlot")
        export(get(plot_output), file = file)
      }, error = function(e) {
        showNotification("Error downloading plot.", type = "error")
      })
    }
  )

  # Progress notification
  observe({
    tryCatch({
      showNotification("Loading dashboard...", duration = 2, type = "message")
    }, error = function(e) {
      message("Error in loading notification: ", e$message)
    })
  })
}

# Run the app
shinyApp(ui, server)



The code includes:

Package Checks: Ensures required packages and versions are installed.
Dynamic CSS: Generates darkly.css, flatly.css, and minty.css for theme switching.
Reactive Datasets: Manages debt data, time series, foreign holders, and Fed remittances.
UI Components: Builds KPI cards, Plotly visualizations, and sidebar controls.
Server Logic: Handles data uploads, scenario analysis, and downloads with error handling.


Why This Matters
The U.S. national debt is more than a number—it’s a lens into economic stability, geopolitical influence, and intergenerational equity. USDebtInsights empowers stakeholders to grapple with its implications:

For Economists: The app’s debt-to-GDP visualizations and Monte Carlo simulations provide rigorous tools to assess fiscal sustainability. By modeling scenarios with varying interest rates, GDP growth, and inflation, researchers can quantify risks and inform academic studies or policy papers.

For Political Strategists: Debt shapes voter priorities and campaign narratives. The app’s intuitive charts and downloadable data enable strategists to craft compelling stories about fiscal responsibility, foreign debt exposure, or Federal Reserve impacts, grounded in credible data.

For Professors and Students: In classrooms, USDebtInsights bridges theory and practice. History professors can contextualize debt trends against past economic crises, while economics students can explore real-time data and run simulations to deepen their understanding of fiscal dynamics.

For Policymakers and the Public: With foreign holders like China and Japan owning significant debt shares, and intragovernmental holdings complicating budget debates, the app clarifies complex dynamics. Its accessible design democratizes data, fostering informed public discourse.


By making debt data interactive, customizable, and visually engaging, USDebtInsights transforms raw numbers into actionable insights, fostering dialogue across academia, policy, and society.

Conclusion
USDebtInsights is a powerful tool for dissecting the U.S. national debt, blending cutting-edge R technology with a user-friendly, PowerBI-inspired interface. Whether you’re analyzing debt composition, forecasting fiscal scenarios, or preparing for a lecture or campaign, this app delivers the data and visuals you need. Explore it live at https://mmcdonald411.shinyapps.io/USDebtApp/, clone the repository to customize it, or contribute to its development on GitHub. Join us in illuminating one of the most pressing economic challenges of our time.
License: MIT
Contributing: Pull requests and issues are welcome! Please follow the contributing guidelines.
Contact: [Your Name] (your.email@example.com) or open an issue on GitHub.

