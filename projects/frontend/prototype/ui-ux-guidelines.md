# UI/UX Design Guidelines - Portfolio App

## Overview

This document provides comprehensive UI/UX guidelines for the Portfolio App, including design system specifications, component patterns, interaction guidelines, and accessibility standards.

---

## Design Principles

### 1. Simplicity

- Keep interfaces clean and uncluttered
- Focus on content, not decoration
- Remove unnecessary elements
- Use progressive disclosure for complex features

### 2. Consistency

- Maintain consistent patterns across the application
- Use design system components
- Follow established interaction patterns
- Ensure visual consistency (colors, typography, spacing)

### 3. Clarity

- Use clear, concise language
- Provide helpful error messages
- Make actions and outcomes obvious
- Use visual hierarchy effectively

### 4. Accessibility

- WCAG 2.1 Level AA compliance minimum
- Keyboard navigation support
- Screen reader compatibility
- Sufficient color contrast
- Touch-friendly targets (min 44x44px)

### 5. Performance

- Optimize images and assets
- Minimize loading times
- Provide immediate feedback
- Use skeleton screens for loading states

---

## Design System

### Color Palette

**Primary Colors**

- **Primary Blue**: `#1890ff` - Primary actions, links, highlights
- **Primary Blue (Hover)**: `#40a9ff` - Hover states
- **Primary Blue (Active)**: `#096dd9` - Active/pressed states

**Secondary Colors**

- **Success Green**: `#52c41a` - Success messages, confirmations
- **Warning Orange**: `#faad14` - Warnings, alerts
- **Error Red**: `#f5222d` - Errors, destructive actions
- **Info Cyan**: `#13c2c2` - Informational messages

**Neutral Colors**

- **Background**: `#ffffff` - Main background
- **Surface**: `#fafafa` - Card backgrounds
- **Border**: `#d9d9d9` - Borders, dividers
- **Text Primary**: `#000000d9` (rgba(0,0,0,0.85))
- **Text Secondary**: `#00000073` (rgba(0,0,0,0.45))
- **Text Disabled**: `#00000040` (rgba(0,0,0,0.25))

**Dark Mode Colors** (Phase 1)

- **Background**: `#141414`
- **Surface**: `#1f1f1f`
- **Border**: `#434343`
- **Text Primary**: `#ffffffd9` (rgba(255,255,255,0.85))
- **Text Secondary**: `#ffffff73` (rgba(255,255,255,0.45))

### Typography

**Font Family**

- Primary: `Inter, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif`
- Monospace: `'Fira Code', 'Consolas', 'Monaco', monospace`

**Type Scale**

- **H1**: 32px / 40px (2rem / 2.5rem) - Page titles
- **H2**: 28px / 36px (1.75rem / 2.25rem) - Section headings
- **H3**: 24px / 32px (1.5rem / 2rem) - Subsection headings
- **H4**: 20px / 28px (1.25rem / 1.75rem) - Card titles
- **Body Large**: 16px / 24px (1rem / 1.5rem) - Large body text
- **Body**: 14px / 22px (0.875rem / 1.375rem) - Default body text
- **Body Small**: 12px / 20px (0.75rem / 1.25rem) - Small text, captions

**Font Weights**

- Regular: 400
- Medium: 500
- Semibold: 600
- Bold: 700

### Spacing System

Based on 4px grid system:

- **xxs**: 4px (0.25rem)
- **xs**: 8px (0.5rem)
- **sm**: 12px (0.75rem)
- **md**: 16px (1rem)
- **lg**: 24px (1.5rem)
- **xl**: 32px (2rem)
- **xxl**: 48px (3rem)
- **xxxl**: 64px (4rem)

**Usage Guidelines**:

- Use `md` (16px) as default spacing
- Use `lg` (24px) between sections
- Use `xl` (32px) between major page sections
- Use `xs` (8px) for tight groupings
- Use `xxl` (48px) for page padding

### Breakpoints

**Responsive Breakpoints**

- **Mobile**: 0-767px
- **Tablet**: 768px-1023px
- **Desktop**: 1024px+
- **Large Desktop**: 1440px+

**Container Widths**

- Mobile: 100% with 16px padding
- Tablet: 768px with 24px padding
- Desktop: 1200px with 32px padding

### Elevation (Shadows)

**Shadow System**

- **Level 1**: `0 2px 8px rgba(0, 0, 0, 0.06)` - Cards, dropdowns
- **Level 2**: `0 4px 12px rgba(0, 0, 0, 0.1)` - Elevated cards, modals
- **Level 3**: `0 8px 16px rgba(0, 0, 0, 0.12)` - Popovers, tooltips
- **Level 4**: `0 12px 24px rgba(0, 0, 0, 0.15)` - Modals, overlays

---

## Component Guidelines

### Buttons

**Primary Button**

- Use for main action on page
- Background: Primary Blue
- Text: White
- Border radius: 6px
- Padding: 8px 16px (small), 12px 24px (default), 16px 32px (large)
- Only one primary button per section

**Secondary Button**

- Use for secondary actions
- Background: Transparent
- Border: 1px solid Primary Blue
- Text: Primary Blue
- Same sizing as primary

**Text Button**

- Use for tertiary actions
- Background: Transparent
- No border
- Text: Primary Blue
- Lower visual weight

**Danger Button**

- Use for destructive actions
- Background: Error Red
- Requires confirmation for critical actions

**Button States**

- Default: Normal appearance
- Hover: Slightly lighter/darker
- Active: Pressed appearance
- Disabled: 50% opacity, no pointer
- Loading: Spinner with disabled state

### Forms

**Input Fields**

- Height: 40px (default)
- Border: 1px solid Border color
- Border radius: 6px
- Padding: 8px 12px
- Font size: 14px

**Input States**

- Default: Border color
- Focus: Primary Blue border, box-shadow
- Error: Error Red border
- Disabled: Gray background, cursor not-allowed
- Success: Success Green border (optional)

**Labels**

- Position: Above input
- Font size: 14px
- Font weight: 500
- Margin bottom: 8px
- Required indicator: Red asterisk

**Helper Text**

- Position: Below input
- Font size: 12px
- Color: Text Secondary
- Error text: Error Red

**Validation**

- Show errors after blur or submit
- Inline validation for real-time feedback
- Clear error messages explaining the issue
- Success indicators when appropriate

### Cards

**Standard Card**

- Background: Surface color
- Border: 1px solid Border color
- Border radius: 8px
- Padding: 16px (mobile), 24px (desktop)
- Box shadow: Level 1

**Hover Card**

- Transforms on hover: `translateY(-4px)`
- Box shadow increases to Level 2
- Smooth transition: 0.3s ease

**Card Content**

- Title: H4 (20px)
- Description: Body (14px)
- Meta info: Body Small (12px)
- Actions: Right-aligned or bottom

### Navigation

**Header Navigation**

- Fixed position at top
- Height: 64px
- Background: White
- Border bottom: 1px solid Border
- Box shadow: Level 1
- Sticky on scroll

**Mobile Navigation**

- Hamburger icon (24x24px)
- Slide-out drawer from right
- Overlay with backdrop
- Smooth animation: 0.3s ease
- Swipe to close gesture

**Navigation Links**

- Active state: Primary Blue color + underline
- Hover state: Primary Blue color
- Default state: Text Primary color

### Modals

**Modal Structure**

- Centered on screen
- Max width: 520px (default), 800px (large)
- Background: White
- Border radius: 8px
- Box shadow: Level 4
- Backdrop: rgba(0, 0, 0, 0.45)

**Modal Content**

- Header: H3 with close button
- Body: Scrollable if needed
- Footer: Actions right-aligned
- Padding: 24px

**Modal Behavior**

- Opens with fade-in animation
- Focus trap inside modal
- ESC key to close
- Click backdrop to close
- Scroll locked on body

### Tooltips

**Tooltip Appearance**

- Background: rgba(0, 0, 0, 0.85)
- Text: White
- Font size: 12px
- Padding: 4px 8px
- Border radius: 4px
- Max width: 250px

**Tooltip Behavior**

- Shows on hover (desktop)
- Shows on focus (keyboard)
- Shows on touch hold (mobile)
- Position: Auto (top, bottom, left, right)
- Arrow pointing to trigger element

### Loading States

**Skeleton Screens**

- Use for content placeholders
- Animated shimmer effect
- Match layout of actual content
- Gray gradient animation

**Spinners**

- Use for actions in progress
- Size: 16px (small), 24px (default), 32px (large)
- Color: Primary Blue
- Circular spinner with animation

**Progress Bars**

- Use for determinate progress
- Height: 4px (thin), 8px (default)
- Background: Border color
- Foreground: Primary Blue

---

## Interaction Patterns

### Feedback

**Success Feedback**

- Green checkmark icon
- Success message
- Toast notification (auto-dismiss after 3s)
- Confetti animation for major achievements (optional)

**Error Feedback**

- Red X icon
- Clear error message explaining issue
- Suggest resolution if possible
- Persistent until user dismisses or resolves

**Information Feedback**

- Blue info icon
- Informational message
- Auto-dismiss after 5s or user dismiss

**Warning Feedback**

- Orange warning icon
- Warning message
- Requires user acknowledgment

### Animations

**Transition Durations**

- Fast: 0.15s - Hover states
- Normal: 0.3s - Most transitions
- Slow: 0.5s - Large movements

**Easing Functions**

- Ease-in-out: Default for most animations
- Ease-out: Elements entering
- Ease-in: Elements exiting

**Animation Guidelines**

- Keep animations subtle and purposeful
- Respect prefers-reduced-motion
- Use transform and opacity for performance
- Avoid animating width/height directly

### Drag and Drop

**Draggable Items**

- Cursor: grab (grabbable), grabbing (dragging)
- Visual feedback: Lift with shadow
- Ghost image during drag
- Drop zones highlighted

**Drop Zones**

- Dotted border when drag active
- Background color change on hover
- Clear indication of valid/invalid drops
- Smooth reordering animation

---

## Accessibility Guidelines

### WCAG 2.1 Level AA Requirements

**Perceivable**

- Color contrast: 4.5:1 for normal text, 3:1 for large text
- Alt text for all images
- Captions for videos
- Audio descriptions where needed

**Operable**

- All functionality keyboard accessible
- No keyboard traps
- Skip navigation link
- Focus indicators visible and clear
- Touch targets minimum 44x44px

**Understandable**

- Clear, simple language
- Consistent navigation
- Predictable interactions
- Error suggestions provided
- Labels and instructions clear

**Robust**

- Valid HTML
- ARIA labels where needed
- Semantic HTML elements
- Compatible with assistive technologies

### Keyboard Navigation

**Tab Order**

- Logical tab order matching visual order
- Skip to main content link
- All interactive elements focusable
- Focus visible on all elements

**Keyboard Shortcuts**

- ESC: Close modals, dropdowns, tooltips
- Enter: Activate buttons, submit forms
- Space: Toggle checkboxes, radio buttons
- Arrow keys: Navigate dropdowns, tabs

### Screen Reader Support

**ARIA Labels**

- Buttons with icons only: aria-label
- Form inputs: aria-describedby for helper text
- Error messages: aria-invalid and aria-errormessage
- Live regions: aria-live for dynamic content

**Semantic HTML**

- Use `<button>` for buttons
- Use `<a>` for links
- Use `<nav>` for navigation
- Use `<main>` for main content
- Use `<header>`, `<footer>`, `<aside>` appropriately

---

## Mobile-Specific Guidelines

### Touch Targets

- Minimum size: 44x44px
- Spacing between targets: 8px minimum
- Larger targets for primary actions

### Mobile Gestures

- Swipe: Navigate, dismiss
- Long press: Open context menu
- Pull to refresh: Refresh content
- Pinch to zoom: Image galleries

### Mobile Navigation

- Bottom navigation for primary actions
- Hamburger menu for secondary navigation
- Floating action button for primary action
- Thumb-friendly zones for frequent actions

### Mobile Forms

- Large input fields (min 44px height)
- Appropriate input types (email, tel, number)
- Autofocus first field (optional)
- Minimize typing with selects and toggles

---

## Content Guidelines

### Writing Style

- Use active voice
- Write short, scannable sentences
- Avoid jargon and technical terms
- Be conversational but professional

### Microcopy

- Button labels: Action-oriented ("Save Changes", not "Submit")
- Error messages: Explain problem and solution
- Empty states: Helpful and encouraging
- Success messages: Positive and specific

### Tone of Voice

- Friendly and approachable
- Professional but not stuffy
- Encouraging and supportive
- Clear and direct

---

## Design Handoff Checklist

### Designer Provides

- [ ] High-fidelity mockups for all screens
- [ ] Interactive prototype with flows
- [ ] Design specifications document
- [ ] Exported assets (icons, images)
- [ ] Design system documentation
- [ ] Responsive breakpoint designs
- [ ] Error and empty state designs

### Developer Receives

- [ ] Access to design files (Figma)
- [ ] Exported assets in appropriate formats
- [ ] Design tokens (colors, spacing, typography)
- [ ] Component specifications
- [ ] Interaction and animation specs
- [ ] Accessibility requirements

---

## Quality Assurance

### Design QA Checklist

- [ ] Matches design specifications
- [ ] Responsive across all breakpoints
- [ ] Consistent spacing and alignment
- [ ] Correct colors and typography
- [ ] Proper hover and focus states
- [ ] Smooth animations and transitions
- [ ] Accessible (keyboard, screen reader)
- [ ] Touch-friendly on mobile
- [ ] Loading and error states work
- [ ] Empty states implemented

---

**Document Status**: ✅ Complete  
**Last Updated**: 2025-11-12  
**Version**: 1.0
