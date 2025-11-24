# CLAUDE.md - Digital Finishing Widget Repository Guide

## Repository Overview

This repository contains data and assets for a **Digital Finishing Equipment Selection System**. It serves as a comprehensive database and visual reference for comparing digital label finishing machinery from various OEMs (Original Equipment Manufacturers).

**Repository Name:** Digital-Finishing-Widget
**Primary Purpose:** Product selection, configuration, and comparison tool for digital finishing equipment
**Last Updated:** November 2025

---

## Repository Structure

```
Digital-Finishing-Widget/
├── widget_minimal.csv              # Main equipment database (63 machines)
├── machine_crops_webp.zip          # Component images (9 files)
├── condensed files/                # Configuration visualization images (16 files)
│   ├── Area Locator *.webp        # Machine configuration locator images
│   ├── *q2.3.4.5.webp            # Question-based navigation images
│   └── Application based Engineering Solutions.webp
└── CLAUDE.md                       # This file
```

---

## Core Data Structure

### Primary Data File: `widget_minimal.csv`

This CSV file is the **central database** containing detailed specifications for 63 digital finishing machines from 25+ manufacturers.

#### Column Schema

| Column Name | Type | Description | Values |
|-------------|------|-------------|---------|
| `OEM` | String | Manufacturer name | AB Graphics, Graphotronics, GM Finishing, etc. |
| `Model` | String | Equipment model name/number | Digicon Series 3, DCL2 Modular, etc. |
| `SQFT + Working Area` | Decimal | Machine footprint in square feet | 46.2 - 553 sq ft |
| `Web Widths (mm)` | String (CSV) | Supported widths in millimeters | "350 mm, 430 mm, 540 mm" |
| `Web Widths (inches)` | String (CSV) | Supported widths in inches | "13.78", 16.93", 21.26"" |
| `Corona Treater` | Boolean | Corona treatment capability | Yes/No/Optional |
| `Optional Print Station` | String | Print technology available | Flexo/Inkjet/No |
| `Cold Foil` | Boolean | Cold foil application | Yes/No/Optional |
| `Hot Foil` | Boolean | Hot foil stamping | Yes/No/Optional |
| `Digital Embellishment` | Boolean | Digital embellishment capability | Yes/No/Optional |
| `Lamination` | Boolean | Lamination support | Yes/No/Optional |
| `UV / LED Curing` | Boolean | UV or LED curing system | Yes/No |
| `Embossing` | Boolean | Embossing capability | Yes/No/Optional |
| `Full Rotary` | Boolean | Full rotary die cutting | Yes/No |
| `Semi-Rotary` | Boolean | Semi-rotary die cutting | Yes/No |
| `Knife / Plotter` | Boolean | Knife/plotter cutting | Yes/No |
| `Laser` | Boolean | Laser cutting/finishing | Yes/No |
| `Slitting` | Boolean | Slitting capability | Yes/No/Optional |
| `# Product Rewinds` | Integer | Number of rewind stations | 1, 2, or 3 |
| `Turret Rewind Add-On` | String | Turret rewind availability | Yes/No/Optional |
| `Inspection` | String | Inspection system availability | Optional (mostly) |

#### Data Quality Notes

- **Empty Cells:** Row 14 (Gonderflex Rotoworx 330) has an empty "Embossing" field
- **Boolean Values:** Use "Yes", "No", or "Optional" (not true/false)
- **CSV Fields:** Web widths stored as comma-separated strings with units
- **Decimal Precision:** SQFT values use 1 decimal place

---

## Visual Assets

### 1. Condensed Files Directory

Contains 16 WebP images organized into categories:

#### Area Locator Images (Configuration Types)
- `Area Locator Ultra Compact.webp` - Ultra compact configuration
- `Area Locator Compact.webp` - Compact configuration
- `Area Locator Modular (1).webp` - Modular configuration
- `Area Locator Full Converting (7).webp` - Full converting configuration

#### Question-Based Navigation
- `ultra comp q2.3.4.5.webp` - Ultra compact decision tree
- `Compact q2.3.4.webp` - Compact decision tree
- `compact q5.6.webp` - Compact alternate path
- `Modular q2.3.4.5.webp` - Modular decision tree
- `Modular q6.webp` - Modular alternate path
- `Full Conv q2.webp`, `q3.4.webp`, `q5.6.webp` - Full converting paths

#### Special Images
- `No Ultra comp.webp` - When ultra compact isn't available
- `No compact Option.webp` - When compact isn't available
- `UC 6.webp` - Ultra compact specific view
- `Application based Engineering Solutions.webp` - Overview diagram

### 2. Machine Crops Archive

`machine_crops_webp.zip` contains 9 component images (285KB total):
- `CRA.webp`, `CFA.webp`, `CP.webp` - Component types
- `MR.webp`, `MAL.webp` - Module types
- `CR.webp`, `CF.webp`, `CX.webp` - Configuration types
- `UC 6.webp` - Ultra compact view (duplicate)

**Note:** Archive appears to use abbreviations (CRA, CFA, CP, MR, MAL, CR, CF, CX)

---

## Development Workflows

### Working with Equipment Data

#### Reading the CSV
```bash
# View first 10 machines
head -10 widget_minimal.csv

# Count total machines (excluding header)
wc -l widget_minimal.csv  # Should show 64 (63 machines + 1 header)

# Search for specific OEM
grep "Graphotronics" widget_minimal.csv

# Find machines with laser capability
grep "Yes" widget_minimal.csv | grep "Laser"
```

#### Data Validation Checklist
When modifying `widget_minimal.csv`:

1. **Preserve Header:** First row must remain intact
2. **Boolean Consistency:** Use only "Yes", "No", or "Optional"
3. **Units:** Always include units in web width fields (mm and inches)
4. **Footprint Range:** SQFT should be 40-600 (typical range)
5. **Rewinds:** Must be 1, 2, or 3 (integer only)
6. **CSV Escaping:** Use quotes for fields containing commas

#### Common Data Operations

**Filter by Capability:**
```python
# Example: Find all machines with both laser and lamination
import csv

with open('widget_minimal.csv', 'r') as f:
    reader = csv.DictReader(f)
    for row in reader:
        if row['Laser'] == 'Yes' and row['Lamination'] == 'Yes':
            print(f"{row['OEM']} - {row['Model']}")
```

**Size Category Analysis:**
```python
# Categorize by footprint
small = []    # < 150 sqft
medium = []   # 150-300 sqft
large = []    # > 300 sqft

for row in reader:
    sqft = float(row['SQFT + Working Area'])
    if sqft < 150:
        small.append(row)
    elif sqft < 300:
        medium.append(row)
    else:
        large.append(row)
```

### Working with Images

#### Extracting Machine Crops
```bash
# Extract all machine component images
unzip machine_crops_webp.zip -d extracted_components/

# List all images with sizes
ls -lh "condensed files/"

# View specific image (requires image viewer)
# xdg-open "condensed files/Area Locator Compact.webp"
```

#### Image Naming Conventions
- **Area Locator:** Overview/configuration selection images
- **q[numbers]:** Question-based decision tree navigation
- **No [type]:** Negative path indicators
- **Component codes:** CRA, CFA, CP, MR, MAL, CR, CF, CX (abbreviations TBD)

### Git Workflow

#### Branch Strategy
- **Main branch:** Production-ready data and assets
- **Feature branches:** Use `claude/` prefix for AI-assisted work
- **Current branch:** `claude/claude-md-mide9xo5k8uv9jy1-01KWkcarSGXFAULEPiAPmiFL`

#### Commit Guidelines
```bash
# Stage data changes
git add widget_minimal.csv

# Commit with descriptive message
git commit -m "Add 5 new machines from Acme Corp

- Added Acme Model X1, X2, X3, X4, X5
- Updated footprint ranges
- Verified all capability flags"

# Push to feature branch
git push -u origin claude/[branch-name]
```

**Commit Message Format:**
```
<action>: <brief summary>

- Bullet point details
- Changed fields/files
- Data validation notes
```

**Actions:** `Add`, `Update`, `Fix`, `Remove`, `Refactor`

---

## Key Conventions for AI Assistants

### Data Integrity Rules

1. **Never Remove Data:** Only add or update existing records
2. **Validate Before Commit:** Always check CSV syntax after edits
3. **Maintain Units:** Never mix metric/imperial without both
4. **Boolean Strictness:** Only use approved values (Yes/No/Optional)
5. **Preserve Formatting:** Keep CSV structure and column order

### Analysis Best Practices

When analyzing equipment data:

1. **Consider Multiple Factors:** Don't filter on single capability
2. **Check Footprint:** Size constraints are critical
3. **Web Width Ranges:** Parse comma-separated values properly
4. **Optional vs No:** "Optional" means available but not standard
5. **Print Station Types:** Flexo and Inkjet are different technologies

### Common Tasks

#### Adding New Equipment
```csv
OEM,Model,SQFT + Working Area,Web Widths (mm),Web Widths (inches),...
New OEM,New Model,250.5,"350 mm, 450 mm","13.78"", 17.72""",Yes,Flexo,...
```

**Checklist:**
- [ ] OEM name matches existing format (or new)
- [ ] Model name is unique
- [ ] SQFT is calculated/verified
- [ ] Web widths include both mm and inches
- [ ] All 21 columns are populated
- [ ] Boolean values use Yes/No/Optional
- [ ] Number of rewinds is 1-3

#### Comparing Equipment
```python
# Example comparison function
def compare_machines(model1, model2):
    """Compare two machines by model name"""
    # Read CSV
    # Filter to models
    # Compare capabilities
    # Return differences dict
```

#### Generating Reports
- **By OEM:** Group machines by manufacturer
- **By Footprint:** Size-based categorization
- **By Capability Matrix:** Feature comparison table
- **By Price Tier:** If pricing data is added

### Image Asset Management

**Guidelines:**
1. **WebP Format:** All images use WebP for compression
2. **Naming:** Use descriptive names with spaces (current convention)
3. **Resolution:** Maintain consistent quality across area locators
4. **Organization:** Keep decision tree images together
5. **Documentation:** Update this file when adding new image categories

### Error Handling

**Common Issues:**
- **CSV Parsing Errors:** Usually quotes or commas in fields
- **Missing Values:** Check row 14 pattern (empty embossing)
- **Unit Mismatches:** Verify mm/inch conversions
- **Image 404s:** Check file paths with spaces (use quotes)
- **Zip Extraction:** Ensure target directory exists

---

## Domain Knowledge

### Digital Finishing Equipment Landscape

**Market Segments:**
1. **Ultra Compact** (< 150 sqft) - Entry level, limited features
2. **Compact** (150-250 sqft) - Mid-range, good feature set
3. **Modular** (250-400 sqft) - Configurable, expandable
4. **Full Converting** (> 400 sqft) - Industrial, comprehensive

**Key Capabilities:**
- **Corona Treater:** Surface treatment for adhesion
- **Flexo/Inkjet:** Inline printing technologies
- **Foiling:** Cold (pressure) vs Hot (heat) application
- **Die Cutting:** Rotary (continuous) vs Semi-rotary (indexed)
- **Finishing:** UV curing, lamination, embossing

**Leading OEMs:**
- AB Graphics (Digicon series) - High-end, feature-rich
- Graphotronics (DCL2, CF2) - Modular systems
- GM Finishing (DC series) - Compact solutions
- BOBST Group (Digital Master) - Industrial scale

### Selection Criteria

**Primary Factors:**
1. **Web Width:** Material width capacity (8-30 inches typical)
2. **Footprint:** Available floor space
3. **Required Capabilities:** Must-have features vs nice-to-have
4. **Production Volume:** Speed and capacity needs
5. **Budget:** Initial investment and operating costs

**Decision Tree Logic:**
The question-based images (q2.3.4.5, q5.6, etc.) suggest a guided selection process:
- Q2-Q5: Likely capability requirements
- Q6: Possibly budget or advanced features
- Configuration type emerges from answers

---

## Future Development Notes

### Potential Enhancements

1. **Pricing Data:** Add cost columns (requires NDA/sourcing)
2. **Speed Specs:** Add meters/minute or feet/minute
3. **Contact Info:** OEM representative details
4. **Installation:** Lead time, training, support info
5. **User Reviews:** Customer feedback and ratings

### Data Expansion

- **Expand to 100+ machines:** More OEMs and models
- **Add consumables:** Compatible materials, inks, foils
- **Service data:** Maintenance schedules, parts availability
- **Case studies:** Application-specific examples

### Technical Improvements

- **Database Migration:** Move from CSV to SQLite/PostgreSQL
- **API Development:** RESTful API for equipment queries
- **Web Interface:** Interactive selection tool
- **Image Metadata:** Tag images with relevant machine models
- **Search Functionality:** Full-text search across specs

---

## Quick Reference

### File Locations
```
Core Data:     ./widget_minimal.csv
Images:        ./condensed files/*.webp
Components:    ./machine_crops_webp.zip
Documentation: ./CLAUDE.md (this file)
```

### Key Statistics
- **Total Machines:** 63
- **Total OEMs:** 25+
- **Web Width Range:** 5.51" - 30.0" (140mm - 762mm)
- **Footprint Range:** 46.2 - 553 sq ft
- **Image Assets:** 25 (16 + 9 archived)

### Quick Commands
```bash
# Count machines by OEM
cut -d',' -f1 widget_minimal.csv | sort | uniq -c | sort -rn

# Find smallest machines
sort -t',' -k3 -n widget_minimal.csv | head -5

# Find largest web widths
grep -o '[0-9]\+\.[0-9]\+ mm' widget_minimal.csv | sort -n | tail -5

# List all unique capabilities
head -1 widget_minimal.csv | tr ',' '\n' | nl
```

---

## Contact & Support

**Repository Maintainer:** TBD
**Last Updated:** November 2025
**AI Assistant Guidelines:** This document

For questions about:
- **Data accuracy:** Verify with OEM spec sheets
- **Image usage:** Check licensing requirements
- **Feature requests:** Document in issues (if applicable)
- **Corrections:** Submit pull requests with evidence

---

## Revision History

| Version | Date | Changes | Author |
|---------|------|---------|--------|
| 1.0 | 2025-11-24 | Initial CLAUDE.md creation | Claude AI |

---

**End of CLAUDE.md**
