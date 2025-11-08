# CrossFit Workout Builder

A single-page web application for designing and exporting CrossFit workouts with a clean, minimal black-and-white interface.

## Overview

This application provides a drag-and-drop interface for building structured CrossFit workouts. It organizes exercises into workout types (supersets) and exports them to professionally formatted PDFs.

## Features

### Workout Types (Supersets)

The application supports four common CrossFit workout formats:

- **For Time**: Configure rounds and time estimate (e.g., "5x For Time ca. 18 min")
- **AMRAP** (As Many Rounds As Possible): Set duration (e.g., "20 min AMRAP")
- **EMOM** (Every Minute On the Minute): Configure total duration and interval (e.g., "20 min EMOM (every 1 min)")
- **Chipper**: Set time estimate for completing all exercises once

Each superset can be given a custom name and contains its own set of exercises.

### Exercise Catalog

Pre-populated with common CrossFit exercises organized by category:

- **Cardio**: Rowing, Running, Assault Bike, Jump Rope, Burpees
- **Weightlifting**: Deadlift, Back Squat, Front Squat, Clean, Snatch, Clean & Jerk, Overhead Press, Push Press
- **Gymnastics**: Pull-ups, Muscle-ups, Handstand Push-ups, Toes to Bar, Box Jumps, Pistol Squats
- **Core**: Sit-ups, Plank, Russian Twists, V-ups
- **Other**: Wall Balls, Kettlebell Swings, Thrusters, Rest

Each exercise is represented with a unique emoji icon for quick visual identification.

### Drag-and-Drop Interface

**Adding Workout Types:**
- Drag a workout type from the catalog to the builder area
- Drop on empty space to add to the end
- Drop between existing supersets (indicated by black line) to insert at specific position

**Adding Exercises:**
- Drag exercises from the catalog into superset containers
- Drop on empty superset area to append to the end
- Drop between existing exercises (indicated by black line above target) for precise positioning
- Exercises can be moved between different supersets

**Reordering:**
- Drag supersets by their handle (⋮⋮) to reorder the entire workout structure
- Drag exercises by their handle (⋮⋮) to reorder within or move between supersets
- Visual feedback shows exactly where items will be placed

### Compact Single-Line Exercise Layout

Each exercise is displayed in a compact format on a single line:
- Drag handle (⋮⋮)
- **Reps** input field (e.g., "10", "21-15-9")
- Exercise icon and name
- **Weight** input field (e.g., "135", "75 kg")
- Duplicate button (📋) - creates a copy immediately below
- Remove button (🗑️) - deletes the exercise

All input fields are small (50px width) since they typically contain only 2-3 characters.

### Superset Configuration

Each superset has:
- **Title field**: Custom name for the superset (e.g., "Warm-up", "Main WOD", "Finisher")
- **Inline configuration**: Type-specific fields arranged horizontally
  - For Time: [5] x For Time ca. [18] min
  - AMRAP: [20] min AMRAP
  - EMOM: [20] min EMOM (every [1] min)
  - Chipper: Chipper ca. [15] min

### PDF Export

Generates a clean, professional PDF with:
- Workout name and date as header
- Each superset organized as a section with:
  - Superset title (bold, 16pt)
  - Type description (12pt)
  - Tabular exercise layout with columns:
    - **Reps** | **Weight** | **Exercise**
- Larger font sizes (11pt) for readability
- Proper pagination for multi-page workouts

## Technical Implementation

### Technology Stack

- **Pure HTML/CSS/JavaScript** - No frameworks or build tools required
- **jsPDF** - For PDF generation (loaded via CDN)
- **Single file** - Entire application in one self-contained HTML file

### Architecture

**Data Structure:**
```javascript
workoutSupersets = [
  {
    id: 0,
    type: { name: "For Time", fields: [...], defaults: {...}, format: fn },
    title: "Main WOD",
    config: { rounds: "5", timeEstimate: "18" },
    exercises: [
      {
        id: 0,
        name: "Deadlift",
        icon: "⚡",
        reps: "10",
        weight: "225"
      },
      // ... more exercises
    ]
  },
  // ... more supersets
]
```

**Key Design Decisions:**

1. **Superset-based structure**: All exercises must belong to a workout type (superset). This enforces proper CrossFit workout structure.

2. **No sets/rounds on individual exercises**: Round configuration is moved to the superset level, as this is how CrossFit workouts are typically structured (e.g., "3 rounds of X, Y, Z" not "3 sets of X, 2 sets of Y").

3. **Drag state management**: Uses global state variables to track what's being dragged (exercise, superset, or catalog item) to handle different drop behaviors.

4. **Position-based drop targeting**: Calculates mouse position relative to element midpoint to determine whether to insert before or after target.

### Styling Principles

- **Minimal black-and-white design**: No colors except black, white, and subtle grays
- **4px border-radius**: Applied consistently throughout for rounded corners
- **Visual feedback**: Black lines indicate drop positions, opacity changes show dragging state
- **Compact layout**: Single-line exercises, inline configuration fields
- **Responsive catalog**: Extends with page height (`calc(100vh - 280px)`)

### Event Handling

**Drag Events:**
- `dragstart` - Set drag state, add visual feedback
- `dragend` - Clear drag state, remove visual feedback
- `dragover` - Show drop position indicator, prevent default to allow drop
- `dragleave` - Clear drop position indicator
- `drop` - Execute the drop action (add, move, reorder)

**Separate handlers for:**
- Superset type dragging (from catalog to builder)
- Exercise dragging (from catalog to superset)
- Superset reordering (within builder)
- Exercise reordering (within and between supersets)
- Empty container drops (append to end)

## Usage Guide

### Creating a Workout

1. **Add workout types**: Drag "For Time", "AMRAP", "EMOM", or "Chipper" into the builder
2. **Name your supersets**: Click the title field and enter a name (e.g., "Warm-up", "Main WOD")
3. **Configure superset parameters**: Fill in the inline fields (rounds, duration, time estimate)
4. **Add exercises**: Drag exercises from the catalog into the superset containers
5. **Configure exercises**: Enter reps and weight for each exercise
6. **Reorder as needed**: Drag supersets or exercises to adjust workout flow
7. **Export**: Click "Export PDF" to generate a downloadable workout sheet

### Example Workout

**WOD 2024-11-08**

**Warm-up**
10 min AMRAP
- 10 Jump Rope
- 5 Burpees
- 10 Sit-ups

**Main WOD**
5x For Time ca. 18 min
- 10 Deadlift @ 225
- 15 Box Jumps
- 20 Wall Balls @ 20

**Finisher**
Chipper ca. 15 min
- 50 Pull-ups
- 100 Push-ups
- 150 Squats

## Browser Compatibility

- Modern browsers (Chrome, Firefox, Safari, Edge)
- Requires ES6+ support (arrow functions, spread operator, template literals)
- Drag-and-drop API support
- No mobile optimization (desktop-focused workflow)

## Future Enhancements

Potential features for future development:

- **Custom exercises**: Allow users to add their own exercises
- **Workout templates**: Pre-built workout templates (Hero WODs, classic CrossFit benchmarks)
- **Time tracking**: Integration with timer for workout execution
- **Workout history**: Save and load previous workouts (localStorage or backend)
- **Exercise variations**: Track scaling options (RX, scaled, beginner)
- **Rest periods**: Explicit rest interval configuration
- **Notes field**: Add coaching cues or workout notes
- **JSON import/export**: Share workouts as JSON files
- **Print optimization**: CSS print styles for direct printing

## Development Notes

### Adding New Workout Types

To add a new workout type, add an entry to the `supersetTypes` array:

```javascript
{
  name: "New Type",
  fields: ["field1", "field2"],
  defaults: { field1: "", field2: "" },
  format: (config) => `${config.field1} New Type ${config.field2}`
}
```

Then update `renderWorkout()` to include the inline configuration HTML for the new type.

### Adding New Exercises

Add entries to the `exercises` object under the appropriate category:

```javascript
"Category": [
  { name: "Exercise Name", icon: "🏋️" },
  // ...
]
```

### Customizing PDF Layout

Modify the `exportBtn` click handler to adjust:
- Font sizes (`doc.setFontSize()`)
- Positioning (x, y coordinates)
- Spacing between elements
- Table column widths
- Page margins

## File Structure

```
crossfit-builder.html     # Single-file application (all HTML, CSS, JS)
Claude.md                 # This documentation file
```

## License

MIT

