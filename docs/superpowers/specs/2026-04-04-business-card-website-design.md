# American Psycho Business Card — Personal Website

## Overview

A single-page personal website styled as two business cards inspired by the iconic American Psycho aesthetic. The cards are stacked on a dark textured background, and clicking swaps which card is on top. All text is rendered on HTML Canvas to prevent bot crawling.

## Cards

Two business cards at standard 3.5:2 proportions. Cream/bone colored with refined serif typography evoking the film's "Silian Rail" lettering (achieved via Playfair Display, Cormorant Garamond, or similar elegant serif web font).

### Card 1 — Aviary Tech

- BRIAN RICHTER (centered, generous letterspacing)
- Chief Executive Officer
- Aviary Tech
- brian@aviary.tech

### Card 2 — Codex / Defined

- BRIAN RICHTER (centered, generous letterspacing)
- Engineer
- Codex / Defined
- brian@defined.fi

### Card Layout

Both cards share the same internal layout:

- Name at center-top, largest and most prominent, uppercase, letterspaced
- Title directly below the name, smaller
- Company name below the title
- Email at the bottom of the card, smallest text

Subtle text shadow to evoke a letterpress/embossed effect. Thin border or very subtle inset shadow on the card edge for realism.

## Interaction

- Two cards stacked with ~10-15px diagonal offset so both edges are visible
- Click/tap anywhere on the card stack to swap which card is on top
- Smooth CSS transition for the swap animation (the top card slides back, the bottom card comes forward)
- Cursor: pointer over the card area
- Invisible `<a href="mailto:...">` overlays positioned over each card's email area for clickability
- The mailto link updates to match whichever card is currently on top (or both links exist, positioned to their respective cards)

## Background

Dark charcoal base with a subtle texture created via CSS (layered gradients or noise pattern). Evokes the dark wood conference table atmosphere from the film. No external image dependencies.

## Anti-Bot Strategy

- All visible text rendered via HTML Canvas — crawlers see only `<canvas>` elements
- No text nodes, aria-labels, or alt text containing the actual content
- Email links use transparent overlays with the mailto href (acceptable trade-off — email is meant to be contactable)
- `user-select: none` on the card containers

## Technical Constraints

- Single `index.html` file, fully self-contained
- One external dependency: Google Fonts import for the serif typeface
- No JS frameworks — vanilla JavaScript only
- Canvas text drawn after font loads (use `document.fonts.ready`)
- Responsive: cards scale proportionally using viewport units or percentage-based sizing, remaining centered on all screen sizes
- Works on mobile: tap to swap cards

## Out of Scope

- No additional pages or navigation
- No analytics or tracking
- No server-side components
