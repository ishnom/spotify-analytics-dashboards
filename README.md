# Spotify Listening Analytics Dashboard

## Project Overview
This project analyzes personal Spotify listening history using interactive Power BI dashboards.  
The goal is to understand listening behavior across **albums, artists, tracks, time patterns, and platforms** through dynamic visuals, drill-downs, and drill-through analysis.

The project consists of **three main dashboards**:
1. Overview Dashboard  
2. Listening Pattern Dashboard  
3. Spotify Listening Insight (Drill-Down & Drill-Through)

---

## 1. Overview Dashboard

The Overview Dashboard is divided into **three analytical sections**:
- Albums
- Artists
- Tracks

### Albums Analysis
- Total Albums
- Albums Played Over Time (Year-wise trend)
- Identification of **Minimum and Maximum Album Years**  
  - Minimum: 2014  
  - Maximum: 2020 and 2022
- Year-over-Year comparison (Latest Year vs Previous Year)
- Weekday vs Weekend album listening behavior
- Top 5 Albums by play frequency

### Artists Analysis
- Total Artists
- Artists Played Over Time
- Latest Year vs Previous Year artist comparison
- Weekday vs Weekend listening analysis
- Top 5 Artists

### Tracks Analysis
- Total Tracks
- Tracks Played Over Time
- Latest Year vs Previous Year track comparison
- Weekday vs Weekend listening pattern
- Top 5 Tracks

### Slicers
- Platform selection
- Shuffle mode
- Repeat mode
- Year filter

All visuals respond dynamically to slicer selections.

---

## 2. Listening Pattern Dashboard

This dashboard focuses on **time-based and behavioral insights**.

### Key Visuals
- Heat Map: Listening Hours vs Days of the Week
- Scatter Plot:  
  - Average Listening Time vs Track Frequency  
  - Highly interactive and dynamically filtered

### Insights
- Second quarter shows the **highest track frequency and highest average listening time**
- Dynamic highlighting based on slicers
- Year, Shuffle Mode, and Listening Scale slicers enable deep exploration

---

## 3. Spotify Listening Insight Dashboard (Drill-Down & Drill-Through)

This dashboard implements **hierarchical analysis and detailed exploration**.

### Metrics Included
- Total Albums
- Total Artists
- Total Tracks
- Total Milliseconds Played
- Average Listening Time (Minutes)

### Drill-Down Hierarchy
- Album → Artist → Track

### Drill-Through Features
- Drill-through enabled using:
  - Total Albums
  - Total Artists
- Enables detailed inspection of specific listening segments

---

## Data Model & Measures

### Date Table
Custom date table includes:
- Date
- Day Name
- Day Number
- Weekday / Weekend
- Year

### Calculated Measures
- Listening Time (Minutes)
- Track Frequency
- Latest Year Albums / Artists / Tracks
- Previous Year Albums / Artists / Tracks
- Min-Max Album Years
- Year-over-Year Comparisons

### Spotify History Table
Contains listening records used to derive all analytical measures and trends.

---

## Tools & Technologies
- Power BI
- DAX (Time Intelligence, Comparison Measures)
- Data Modeling
- Interactive Visual Design

---

## Key Outcomes
- Clear understanding of listening behavior over time
- Identification of peak listening periods
- Platform and mode-based listening insights
- Deep drill-through analysis from summary to granular level
