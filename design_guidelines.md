# Todo List App Design Guidelines

## Design Approach
**Reference-Based with System Foundation**: Drawing from Notion and Linear's clean productivity aesthetics, adapted with the specified light green theme. Focus on clarity, efficiency, and calm productivity.

## Core Design Philosophy
Simple, focused task management with a refreshing light green aesthetic that reduces cognitive load while maintaining visual interest.

## Typography System
- **Primary Font**: Inter or SF Pro Display via Google Fonts
- **Headings**: 
  - App title/logo: text-2xl font-semibold
  - Section headers: text-lg font-medium
  - Todo titles: text-base font-medium
- **Body Text**: text-sm for descriptions and metadata
- **Line Height**: leading-relaxed for readability

## Color Palette (Light Green Theme)
- **Primary Green**: Light sage/mint green (#E8F5E9 to #C8E6C9 range)
- **Accent Green**: Medium green for interactive elements (#66BB6A)
- **Text**: Charcoal gray (#2C3E50) for primary, lighter gray for secondary
- **Backgrounds**: White base with light green tints for sections
- **Status Colors**: Soft green for completed, neutral gray for pending

## Layout & Spacing System
**Spacing Units**: Consistent use of Tailwind units 2, 4, 6, and 8
- Container padding: p-6 to p-8
- Section gaps: space-y-4 to space-y-6
- Card padding: p-4
- Button padding: px-4 py-2

**Layout Structure**:
- Max-width container: max-w-4xl mx-auto (centered, focused width)
- Single-column task list for clarity
- Sticky header with user info and logout

## Component Library

### Authentication Screen
- Centered card (max-w-md) on light green gradient background
- App logo/title at top
- "Login with Replit" button (prominent, uses Replit branding)
- Minimal, welcoming design with brief app description

### Header Bar
- Sticky top navigation (sticky top-0 z-10)
- Light green background with subtle border
- Left: App name/logo
- Right: Username display + logout button
- Height: h-16 with flex items-center

### Todo Input Form
- Prominent position at top of main content
- Input fields with light green focus rings
- Title input: Single line, larger text
- Description textarea: Optional, 2-3 rows
- "Add Task" button: Accent green with hover state
- Form grouped in card with subtle shadow

### Todo Item Card
- Clean card design with rounded corners (rounded-lg)
- Padding: p-4
- Border-left accent: 4px solid green when active, gray when complete
- Layout:
  - Checkbox (left): Custom styled, large click area
  - Content (center): Title (bold) + description (muted) stacked
  - Actions (right): Edit and delete icon buttons
- Completed state: Reduced opacity, strikethrough title

### Todo List Container
- Vertical stack with consistent gaps (space-y-3)
- Group by status: Active tasks above, completed tasks below with divider
- Empty state: Centered message with light icon when no tasks

### Buttons
- Primary: Solid accent green with white text
- Secondary: Outlined with green border
- Icon buttons: Ghost style with hover background
- All buttons: rounded-md, smooth transitions
- Consistent height: h-10 for standard buttons

### Status Indicators
- Checkbox: Large (w-5 h-5), rounded, transforms when checked
- Completion badge: Small pill showing "X completed" count
- Visual feedback: Smooth transitions for all state changes

## Interaction Patterns
- **Add Todo**: Click input → type → press Enter or click button
- **Complete**: Click checkbox → smooth strikethrough animation
- **Edit**: Click edit icon → inline editing mode OR modal dialog
- **Delete**: Click delete → simple confirm → smooth fade out
- **Keyboard**: Enter to submit, Escape to cancel edits

## Responsive Behavior
- Mobile (< 768px): Full-width cards, reduced padding (p-4)
- Desktop: Centered max-w-4xl container, comfortable spacing (p-8)
- All touch targets minimum 44px for mobile usability

## Visual Hierarchy
1. Input form (most prominent, always visible)
2. Active/pending todos (primary focus area)
3. Completed todos (secondary, muted)
4. User info and logout (header, always accessible)

## Key UX Principles
- **Immediate feedback**: All actions provide instant visual response
- **Clear states**: Distinct visual treatment for active vs completed
- **Minimal friction**: Quick task entry, single-click completion
- **Calm aesthetics**: Light green theme promotes focus without distraction
- **Data confidence**: Auto-save indicators, smooth transitions reassure persistence

## Accessibility
- Semantic HTML: `<form>`, `<button>`, `<input>`, `<label>`
- ARIA labels for icon-only buttons
- Keyboard navigation: Full tab order, Enter/Escape support
- Color contrast: Ensure text meets WCAG AA standards against green backgrounds
- Focus indicators: Visible ring on all interactive elements