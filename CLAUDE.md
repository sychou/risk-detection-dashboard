# Risk Signals Dashboard - Single File HTML Build

Build a risk signals dashboard as a single HTML file with embedded JavaScript and CSS. No frameworks, no external dependencies except Google Fonts.

## Tech Stack

- Plain HTML5
- Vanilla JavaScript (ES6+)
- CSS (no preprocessors)
- Google Fonts: IBM Plex Sans, IBM Plex Mono

## Data

Embed data as a single JavaScript object:

```javascript
const data = {
  baseUrl: 'https://msig.9root.ai/workflow-runs/',
  
  dailyEmails: [
    { date: '2026-01-21', count: 142 },
    { date: '2026-01-22', count: 156 },
    { date: '2026-01-23', count: 128 },
    { date: '2026-01-24', count: 134 },
    { date: '2026-01-25', count: 118 },
    { date: '2026-01-26', count: 145 },
    { date: '2026-01-27', count: 103 },
  ],
  
  alerts: [
    {
      datetime: '2026-01-22T09:14:00',
      runId: '01KFH7E0A71TWYH0230AYG3WP5',
      category: 'Claim Severity & Complexity',
      signal: 'Fatality or Catastrophic Injury',
      rationale: 'Explosion at senior apartments: 1 fatality, injuries, 141 displaced',
      criticality: 'critical'  // 'critical', 'high', 'medium', 'low'
    },
    // ... more alerts
  ]
};
```

Derive all counts and groupings from `data.alerts`:

- Daily signals/highRisk counts: group alerts by date
- Categories and signals: group alerts by category, then by signal
- High risk items: filter alerts where criticality is 'critical' or 'high'

## Components

### Header

Title: "Risk Signal Dashboard", Subtitle: derived from date range in data (e.g., "January 21-27, 2026")

### Financial Impact Assessment

Two-column grid showing quantified and unquantified financial impact:

- **Recovery Potential** (green accent, left column):
  - Extracts dollar amounts from subrogation-related alerts
  - Shows total from quantified signals only (labeled "quantified")
  - Clickable count for quantified signals with criticality badges
  - Clickable count for unquantified signals with criticality badges
  - Click either count to open modal with specific signals

- **Exposure & Reserves Impact** (red accent, right column):
  - Extracts dollar amounts from exposure-related alerts (excess notifications, property losses, etc.)
  - Shows total from quantified signals only (labeled "quantified")
  - Clickable count for quantified signals with criticality badges
  - Clickable count for unquantified signals with criticality badges
  - Click either count to open modal with specific signals

Dollar amounts are extracted from rationale text using regex pattern matching. Amounts with M/B/K suffixes are normalized. Both quantified and unquantified counts display criticality badges (critical/high/medium/low) showing the breakdown of signals by severity.

### Metric Cards (3 columns)

Emails Processed, Signals Detected (clickable), High Risk (clickable, red accent).
- Signals Detected opens modal showing all signals sorted by time (newest first)
- High Risk opens modal showing critical/high alerts sorted by criticality

### Daily Volume Chart

Vertical layout (days top to bottom) with horizontal unit-box bars that wrap:

- Date label: Two lines, e.g. "Jan 22" on first line, "Thu" on second line
- 1 box per email, wrapping to multiple lines if needed
- Each alert corresponds to one email, but not all emails generate alerts
- Box size: 10px × 10px
- 1px gap between boxes
- Colors by criticality (boxes ordered left to right by severity):
  - Critical: #DC2626 (red)
  - High: #EA580C (orange)
  - Medium: #CA8A04 (amber)
  - Low: #525252 (dark gray)
  - No signal: #E5E5E5 (light gray)
- Email count on right of each row

### Signals by Category

Expandable accordion. Categories and signals are derived from the alerts array. Each category expands to show signal types. Signal types are clickable and open a modal with instances.

### Modals

All modals display a subtitle indicating how the content is sorted.

- Signal instances modal: Shows alerts for a signal type sorted by criticality, links to `{data.baseUrl}{runId}`
- All signals modal: Shows all signals sorted by time (newest first)
- High risk modal: Shows critical/high alerts sorted by criticality
- Financial impact modal: Shows quantified/unquantified signals sorted by criticality

### Footer

"Powered by Isomer AI · Real-time claims intelligence"

## Styling

- Background: #F5F5F5, Cards: #FFFFFF, Border: #E5E5E5
- Text: #171717 primary, #737373 secondary, #A3A3A3 muted
- Accent: #DC2626, Signal: #525252
- Border radius: 6px cards, 4px buttons
- Font: IBM Plex Sans for text, IBM Plex Mono for numbers