# The Brief — Design Spec

## Overview

A second section below the business card stack. A typed memo/dossier rendered on canvas with the same bone/cream paper aesthetic and letterpress typography. Scrolls into view below the cards on the same page.

## Layout

The page becomes scrollable. Order from top to bottom:

1. **Business card stack** — existing, centered vertically in the initial viewport
2. **Scroll gap** — breathing room (~30-40vh)
3. **The Brief** — a single large canvas element, wider than the cards (~800px), styled like a typed document on cream paper with the same subtle shadow and border treatment

The body `overflow: hidden` must change to allow scrolling.

## Brief Content

**Header area:**
- "BRIEF" centered at top, letterspaced, small caps
- A thin horizontal rule
- "RE: Brian Richter" left-aligned below

**Body — flowing prose paragraphs, left-aligned:**

Paragraph 1: Aviary Tech — Founder and CEO. Focused on Decentralized Identity. Building tools around W3C Decentralized Identifiers and Verifiable Credentials. Making digital identity self-sovereign and interoperable.

Paragraph 2: Codex / Defined — Software Engineer. Codex indexes real-time data for over seventy million tokens across eighty-plus blockchain networks with sub-second latency. Clients include Coinbase, Uniswap, TradingView, and Farcaster. Defined provides the analytics layer.

Paragraph 3: Originals — Creator and author of the protocol. A minimal system for creating, discovering, and transferring digital assets with cryptographically verifiable provenance. Assets migrate through three layers: private (did:peer), public (did:webvh), and transferable (did:btco). Ownership settles on Bitcoin. No smart contracts, no trusted third parties, no bespoke blockchains.

Paragraph 4: Tease the Praxis Project Proposal as forthcoming. One sentence.

**Tone:** Concise, factual, slightly formal. Like an intelligence brief — not self-promotional, just stating what is.

## Visual Treatment

- Same bone/cream background (#ede8df) as cards
- Same Cormorant SC / Cormorant fonts
- Same letterpress double-pass rendering (dark shadow above, light highlight below)
- Left-aligned body text (not centered — this is a document, not a card)
- "BRIEF" header and "RE:" line use small caps
- Body text in Cormorant regular at readable size (~15-16px)
- Line height generous for readability
- Subtle card shadow matching the business cards
- Thin border matching the cards

## Technical

- Same canvas anti-bot rendering — all text drawn on canvas, nothing in DOM
- Canvas sized to fit content (calculate height based on text wrapping)
- Manual text wrapping needed (canvas doesn't wrap text) — split into lines by measuring width
- Responsive: scales down on mobile like the cards
- Added to the existing index.html — no new files

## Out of Scope

- The Praxis Project Proposal document (future work)
- Interactive elements on the brief
- Links
