# DAX Formulas Documentation - Spotify History Analysis

This document provides detailed documentation for all DAX measures and calculated columns used in the Spotify History Analysis project.

## Table of Contents
- [Date Table](#date-table)
- [Album Measures](#album-measures)
- [Artist Measures](#artist-measures)
- [Track Measures](#track-measures)
- [Calendar Calculations](#calendar-calculations)

---

## Date Table

### Date Table Creation
```dax
Date Table = 
CALENDAR(
    MIN(spotify_history[Track Play Date]),
    MAX(spotify_history[Track Play Date])
)
```

**Purpose**: Creates a continuous date dimension table spanning from the earliest to the latest track play date in the dataset.

**How it works**:
- Uses the `CALENDAR()` function to generate all dates between two boundaries
- `MIN(spotify_history[Track Play Date])` finds the earliest play date
- `MAX(spotify_history[Track Play Date])` finds the latest play date
- Automatically creates a table with a single column named `[Date]`

**Use Case**: Essential for time intelligence calculations and enables proper date-based filtering and analysis.

---

### Week End/Day Classification
```dax
week_end/day = 
IF(WEEKDAY('Date Table'[Date], 2) <= 5, "Weekday", "Weekend")
```

**Purpose**: Categorizes each date as either "Weekday" or "Weekend".

**How it works**:
- `WEEKDAY('Date Table'[Date], 2)` returns the day of the week (Monday=1, Sunday=7)
- Parameter `2` sets Monday as the first day of the week
- Days 1-5 (Monday-Friday) are classified as "Weekday"
- Days 6-7 (Saturday-Sunday) are classified as "Weekend"

**Use Case**: Enables analysis of listening patterns between weekdays and weekends.

---

## Album Measures

### Total Albums
```dax
Total Albums = 
DISTINCTCOUNT(spotify_history[album_name])
```

**Purpose**: Counts the unique number of albums in the current filter context.

**How it works**:
- `DISTINCTCOUNT()` returns the count of distinct/unique values
- Operates within the current filter context (respects slicers, filters, and row context)

**Use Case**: Base measure for understanding album diversity in listening habits. Used in other calculations and can be filtered by date, artist, or other dimensions.

---

### Minmax Album Labels
```dax
Minmax album lables = 
VAR Maxvalue = MAXX(
    ALLSELECTED('Date Table'[Year]),
    CALCULATE(DISTINCTCOUNT(spotify_history[album_name]))
)

VAR Minvalue = MINX(
    ALLSELECTED('Date Table'[Year]),
    CALCULATE(DISTINCTCOUNT(spotify_history[album_name]))
)

VAR Currentvalue = DISTINCTCOUNT(spotify_history[album_name])

RETURN
    IF(Currentvalue == Minvalue || Currentvalue == Maxvalue, Currentvalue, BLANK())
```

**Purpose**: Displays album count values only for the years with minimum and maximum album diversity, hiding all other values.

**How it works**:
1. **Maxvalue Variable**: Iterates through all selected years and finds the maximum distinct album count
   - `ALLSELECTED('Date Table'[Year])` maintains user selections while removing row context
   - `MAXX()` evaluates the expression for each year and returns the maximum

2. **Minvalue Variable**: Similar to Maxvalue but finds the minimum distinct album count using `MINX()`

3. **Currentvalue Variable**: Calculates the distinct album count for the current row/context

4. **RETURN Logic**: 
   - If current value equals either the minimum OR maximum (`||` operator), display it
   - Otherwise, return `BLANK()` to hide the label

**Use Case**: Ideal for highlighting outlier years in visualizations - automatically labels only the most and least diverse listening years without cluttering the chart.

---

### Latest Year Album
```dax
Latest Year Album = 
VAR _latestYear = MAX('Date Table'[Year])

RETURN
CALCULATE([Total Albums], 'Date Table'[Year] = _latestYear)
```

**Purpose**: Returns the total number of distinct albums listened to in the most recent year.

**How it works**:
1. **_latestYear Variable**: Finds the maximum (most recent) year in the current filter context
2. **RETURN**: Evaluates the `[Total Albums]` measure specifically for the latest year
   - `CALCULATE()` modifies the filter context
   - Filters to only include data where Year equals the latest year

**Use Case**: Useful for KPI cards, comparisons with previous years, or tracking current year trends regardless of date filters applied.

---

## Artist Measures

The Artist measures follow the same structural pattern as Album measures, providing parallel analysis for artist-level insights.

### Total Artists
```dax
Total Artists = 
DISTINCTCOUNT(spotify_history[artist_name])
```

**Pattern**: Same as Total Albums, but counts unique artists instead of albums.

---

### Minmax Artist Labels
```dax
Minmax artist labels = 
VAR Maxvalue = MAXX(
    ALLSELECTED('Date Table'[Year]),
    CALCULATE(DISTINCTCOUNT(spotify_history[artist_name]))
)

VAR Minvalue = MINX(
    ALLSELECTED('Date Table'[Year]),
    CALCULATE(DISTINCTCOUNT(spotify_history[artist_name]))
)

VAR Currentvalue = DISTINCTCOUNT(spotify_history[artist_name])

RETURN
    IF(Currentvalue == Minvalue || Currentvalue == Maxvalue, Currentvalue, BLANK())
```

**Pattern**: Same logic as Minmax Album Labels, identifying years with highest and lowest artist diversity.

---

### Latest Year Artist
```dax
Latest Year Artist = 
VAR _latestYear = MAX('Date Table'[Year])

RETURN
CALCULATE([Total Artists], 'Date Table'[Year] = _latestYear)
```

**Pattern**: Same structure as Latest Year Album, returning artist count for the most recent year.

---

## Track Measures

The Track measures follow the same structural pattern as Album and Artist measures, providing granular track-level analysis.

### Total Tracks
```dax
Total Tracks = 
DISTINCTCOUNT(spotify_history[track_name])
```

**Pattern**: Same as Total Albums and Total Artists, but counts unique tracks played.

---

### Minmax Track Labels
```dax
Minmax track labels = 
VAR Maxvalue = MAXX(
    ALLSELECTED('Date Table'[Year]),
    CALCULATE(DISTINCTCOUNT(spotify_history[track_name]))
)

VAR Minvalue = MINX(
    ALLSELECTED('Date Table'[Year]),
    CALCULATE(DISTINCTCOUNT(spotify_history[track_name]))
)

VAR Currentvalue = DISTINCTCOUNT(spotify_history[track_name])

RETURN
    IF(Currentvalue == Minvalue || Currentvalue == Maxvalue, Currentvalue, BLANK())
```

**Pattern**: Same logic as other Minmax measures, highlighting years with extreme track diversity.

---

### Latest Year Track
```dax
Latest Year Track = 
VAR _latestYear = MAX('Date Table'[Year])

RETURN
CALCULATE([Total Tracks], 'Date Table'[Year] = _latestYear)
```

**Pattern**: Same structure, returning track count for the most recent year.

---

## Key DAX Concepts Used

### Functions Reference

| Function | Purpose | Usage in Project |
|----------|---------|------------------|
| `DISTINCTCOUNT()` | Counts unique values | Counting unique albums, artists, and tracks |
| `CALCULATE()` | Modifies filter context | Filtering to specific years or conditions |
| `MAXX()` / `MINX()` | Iterates and finds max/min | Finding highest/lowest diversity years |
| `ALLSELECTED()` | Removes row context, keeps user filters | Comparing current row to all visible data |
| `MAX()` / `MIN()` | Returns maximum/minimum value | Finding latest year or date boundaries |
| `CALENDAR()` | Generates date table | Creating continuous date dimension |
| `WEEKDAY()` | Returns day of week number | Classifying weekdays vs weekends |
| `IF()` | Conditional logic | Showing/hiding labels, categorizing days |
| `BLANK()` | Returns blank value | Hiding unnecessary labels in charts |

---

### Pattern: Measure Reusability

All three measure categories (Albums, Artists, Tracks) follow an identical pattern, which:
- **Reduces code duplication**: Same logic applied to different columns
- **Ensures consistency**: All metrics behave the same way
- **Simplifies maintenance**: Fix once, applies everywhere
- **Enables comparative analysis**: Parallel measures can be easily compared

---

### Filter Context vs Row Context

**Filter Context** (used in most measures):
- Determined by slicers, filters, and visual axes
- Affects what data is visible to calculations
- Modified using `CALCULATE()`

**Row Context** (used in MAXX/MINX):
- Iterates row-by-row through a table
- Required for X-aggregation functions
- Removed by `ALLSELECTED()` to enable cross-row comparisons

---

## Best Practices Demonstrated

1. **Variable Usage**: Using `VAR` makes formulas more readable and efficient (values calculated once)
2. **Naming Conventions**: Clear, descriptive names (Total Albums, Latest Year Album)
3. **Measure Independence**: Base measures like `[Total Albums]` can be reused in other calculations
4. **Smart Labeling**: Minmax measures reduce chart clutter by showing only relevant data points
5. **Date Table Pattern**: Separate date dimension enables proper time intelligence
6. **Consistent Structure**: Repeating patterns across Albums/Artists/Tracks for predictability

---

## Usage Recommendations

### For Visualizations
- Use **Total** measures in cards, tables, and basic charts
- Use **Minmax Labels** measures in line/column charts to automatically highlight extremes
- Use **Latest Year** measures in KPI cards for current performance tracking

### For Analysis
- Combine Album/Artist/Track measures to understand listening diversity
- Use Date Table with time slicers for trend analysis
- Filter by week_end/day to compare weekday vs weekend listening habits

---

## Dependencies

These measures require:
- `spotify_history` table with columns: `album_name`, `artist_name`, `track_name`, `Track Play Date`
- `Date Table` dimension with `Year` column
- Proper relationship between tables on date columns

---

*Last Updated: February 5, 2026*
