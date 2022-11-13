---
title: '🪖 Global Military Expenditure Dashboard'
description: 'View military spending across the world.' 
author: 'Nathan States'
date: '2022-10-13'
images: 
  - 'https://images.unsplash.com/photo-1547234843-4ec3943c856c?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=874&q=80' 
categories: ["Dashboards"]
links:
  - icon: rocket-launch
    icon_pack: fa
    name: View
    url: https://nathan-states.shinyapps.io/sipri-dashboard/
  - icon: github
    icon_pack: fa
    name: Source
    url: https://github.com/Nathan-States/SIPRI-Dashboard
---

# Global Military Expenditure Dashboard 

The Global Military Expenditure Dashboard allows users to view military spending across the world using several different metrics, including:

+ Converted USD 
+ Inflation-Adjusted USD 
+ Military Spending in GDP Percentage 
+ Military Spending in Government Percentage
+ Military Spending in Capita Spending 

View the dashboard by clicking [here](https://nathan-states.shinyapps.io/sipri-dashboard/). 
## Data Source 

The data comes from the Stockholm International Peace Research Institute ([SIPRI](https://sipri.org/)), a think tank that researches conflict, armaments, arms control, and disarmament. SIPRI was created by the [Swedish Parliament](https://sipri.org/about/history) in July 1966, who gives them an annual grant to fund their operations. They also receive funding from a variety of governments and NGOs, including the EU, Australian Government, the University of Notre Dame, the Norwegian Ministry, [and many more](https://sipri.org/about/funding-2021). 

You can download the database used for this project [here](https://milex.sipri.org/sipri).

## Photo Credit 

Photo by [Juli Kosolapova](https://unsplash.com/@yuli_superson). 

### Improvements 

Some things I hope to return to in the future to improve the application: 

+ Add additional metrics to the global map slide (GDP percentage, government percentage, and capita amounts). 
+ Improve the color scale of the global map so it's less ugly and progresses smoother. 
+ Fix the axis and tooltip of the comparison slide so that currency and percents are formatted correctly. 
+ Improve the styling as a whole. 
+ Add additional information in the future, specifically arms transfers between countries. 

This application is currently being hosted on [shinyapps.io](https://www.shinyapps.io/) on a free plan. Performance is somewhat limited, and applications are limited to 25 active hours a month. It may run slower on worse computers, and though unlikely, could be deactivated until the end of the month if all 25 active hours are used. 