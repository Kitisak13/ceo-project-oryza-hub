---
name: pbi-custom-timelines
description: Techniques for creating Horizontal Milestones Timelines (Scatter/Line Charts) and Vertical Milestone Timelines (Bar Charts) using native Power BI visuals and dummy DAX positioning measures.
---

# Power BI Custom Timelines & Milestones

Create customized corporate milestone timelines without relying on custom visuals. Techniques cover both **Horizontal Timelines** (using Line/Scatter Charts) and **Vertical Timelines** (using Bar Charts).

---

## 1. Horizontal Milestone Timeline (Line / Scatter Chart)

Create a clean horizontal timeline showing company milestones, product launches, or event dates along an X-axis.

### DAX Positioning & Label Measures

```dax
// Alternate milestone Y positions to prevent label collision (+1, -1, +2, -2)
measure 'Event Y Pos' = 
VAR _EventID = SELECTEDVALUE( 'TeslaTimeline'[Event ID] )
RETURN MOD( _EventID, 2 ) * 2 - 1

measure 'Event Description Label' = 
VAR _Desc = SELECTEDVALUE( 'TeslaTimeline'[Event Description] )
VAR _Date = FORMAT( SELECTEDVALUE( 'TeslaTimeline'[Event Date] ), "MMM YYYY" )
RETURN _Date & " - " & _Desc

measure 'Baseline' = 0
```

### Visual Setup (Line & Clustered Column / Scatter)
- **Shared Axis**: `TeslaTimeline[Event Date]`
- **Column / Line Values**: `[Baseline]` (as a flat center line) and `[Event Y Pos]` (as milestone points above/below baseline).
- **Data Labels**: Enable data labels on `[Event Y Pos]` mapped to `[Event Description Label]`.

---

## 2. Vertical Milestone Timeline (Bar Chart)

Create a vertical step-by-step progress timeline or monthly goal roadmap using a standard horizontal/vertical Bar Chart.

### DAX Measures for Vertical Timeline

```dax
measure 'Line Position' = 0.06 // Fixed dummy bar width for line stem

measure 'Completion Icon' = 
VAR _Pct = SELECTEDVALUE( 'Teams-Monthly-Goals'[Progress] )
RETURN IF( _Pct >= 1, "✔ ", "⏳ " )

measure 'Label Month' = 
VAR _IsCurrent = SELECTEDVALUE( 'dimDate'[IsCurrentPeriod] )
VAR _Month = SELECTEDVALUE( 'dimDate'[Month] )
RETURN IF( _IsCurrent, "📌 " & _Month, _Month )

measure 'Label Goal' = 
VAR _Goal = SELECTEDVALUE( 'Teams-Monthly-Goals'[Goal] )
VAR _Pct = FORMAT( SELECTEDVALUE( 'Teams-Monthly-Goals'[Progress] ), "0%" )
RETURN [Completion Icon] & _Goal & " (" & _Pct & ")"

measure Transparent = "#ffffff00" // Fully transparent color for spacer bars
```

### Visual Setup (Clustered / Stacked Bar Chart)
- **Y-Axis**: `dimDate[Month]`
- **X-Axis**: `[Line Position]` (renders the stem bar) and `[Placeholder Goals]` (renders progress).
- **Custom Formatting**: Apply `Transparent` measure to background gridlines and spacers to create an floating vertical line stem effect.

## Best Practices
- Use `UNICHAR` symbols (e.g. `✔`, `📌`, `⏳`, `▲`, `▼`) in measure strings for clean visual milestone markers.
- Turn off chart axes, gridlines, and backgrounds to make the timeline look like a native Infographic UI.
