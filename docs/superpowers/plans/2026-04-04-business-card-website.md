# Business Card Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-page personal website that renders two American Psycho-style business cards on canvas, stacked and swappable on click.

**Architecture:** Single `index.html` with embedded CSS and JS. Cards rendered on two `<canvas>` elements positioned absolutely within a container. Invisible `<a>` overlays for email links. Click handler swaps z-index and offset positions with CSS transitions.

**Tech Stack:** HTML5 Canvas, vanilla CSS/JS, Google Fonts (Cormorant Garamond)

---

## File Structure

- Create: `index.html` — the entire website (HTML structure, embedded `<style>`, embedded `<script>`)

That's it. One file.

---

### Task 1: HTML Shell with Background and Font Loading

**Files:**
- Create: `index.html`

- [ ] **Step 1: Create the base HTML file**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Brian Richter</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:wght@300;400;500;600&display=swap" rel="stylesheet">
  <style>
    *, *::before, *::after {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
    }

    body {
      min-height: 100vh;
      display: flex;
      align-items: center;
      justify-content: center;
      background-color: #1a1714;
      background-image:
        repeating-linear-gradient(
          90deg,
          transparent,
          transparent 50px,
          rgba(0,0,0,0.03) 50px,
          rgba(0,0,0,0.03) 51px
        ),
        repeating-linear-gradient(
          0deg,
          transparent,
          transparent 30px,
          rgba(0,0,0,0.02) 30px,
          rgba(0,0,0,0.02) 31px
        ),
        linear-gradient(
          135deg,
          #1a1714 0%,
          #1f1b17 25%,
          #1a1714 50%,
          #1c1915 75%,
          #1a1714 100%
        );
      overflow: hidden;
      user-select: none;
      -webkit-user-select: none;
    }
  </style>
</head>
<body>
  <script>
    // Will be filled in subsequent tasks
  </script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify**

Run: `open index.html`
Expected: Dark charcoal page with subtle texture, centered and full viewport. No visible content yet.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add HTML shell with dark textured background"
```

---

### Task 2: Card Container and Canvas Elements

**Files:**
- Modify: `index.html` — add card container HTML between `<body>` and `<script>`, add card CSS

- [ ] **Step 1: Add card container HTML**

Insert before the `<script>` tag:

```html
  <div class="card-stack" id="cardStack">
    <div class="card-wrapper" id="card2wrapper" data-card="2">
      <canvas id="card2" width="700" height="400"></canvas>
      <a class="email-link" id="email2link" href="mailto:brian@defined.fi"></a>
    </div>
    <div class="card-wrapper" id="card1wrapper" data-card="1">
      <canvas id="card1" width="700" height="400"></canvas>
      <a class="email-link" id="email1link" href="mailto:brian@aviary.tech"></a>
    </div>
  </div>
```

Note: card1wrapper is listed second so it renders on top (later in DOM = higher stacking). Card 2 is behind.

- [ ] **Step 2: Add card CSS**

Add inside the `<style>` block:

```css
    .card-stack {
      position: relative;
      width: 700px;
      height: 400px;
      cursor: pointer;
    }

    .card-wrapper {
      position: absolute;
      width: 700px;
      height: 400px;
      transition: transform 0.5s cubic-bezier(0.4, 0, 0.2, 1),
                  box-shadow 0.5s cubic-bezier(0.4, 0, 0.2, 1);
      border-radius: 4px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.3), 0 1px 3px rgba(0,0,0,0.2);
    }

    .card-wrapper canvas {
      display: block;
      width: 100%;
      height: 100%;
      border-radius: 4px;
    }

    /* Back card offset */
    .card-wrapper.back {
      transform: translate(12px, 12px);
      box-shadow: 0 1px 4px rgba(0,0,0,0.2);
    }

    /* Front card */
    .card-wrapper.front {
      transform: translate(-4px, -4px);
      box-shadow: 0 4px 16px rgba(0,0,0,0.4), 0 2px 6px rgba(0,0,0,0.3);
    }

    .email-link {
      position: absolute;
      bottom: 40px;
      left: 50%;
      transform: translateX(-50%);
      width: 260px;
      height: 30px;
      z-index: 10;
      cursor: pointer;
    }

    /* Responsive scaling */
    @media (max-width: 760px) {
      .card-stack,
      .card-wrapper {
        width: 90vw;
        height: calc(90vw * 400 / 700);
      }
    }
```

- [ ] **Step 3: Open in browser and verify**

Run: `open index.html`
Expected: Two stacked rectangles (blank canvases — white/transparent) on the dark background. The cards won't have content yet but should be visible as rectangles.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add card container, canvas elements, and responsive CSS"
```

---

### Task 3: Canvas Rendering Function

**Files:**
- Modify: `index.html` — replace the script block with canvas drawing logic

- [ ] **Step 1: Write the card rendering script**

Replace the `<script>` contents with:

```javascript
    const CARDS = [
      {
        id: 'card1',
        name: 'BRIAN RICHTER',
        title: 'Chief Executive Officer',
        company: 'Aviary Tech',
        email: 'brian@aviary.tech'
      },
      {
        id: 'card2',
        name: 'BRIAN RICHTER',
        title: 'Engineer',
        company: 'Codex / Defined',
        email: 'brian@defined.fi'
      }
    ];

    function drawCard(canvasId, data) {
      const canvas = document.getElementById(canvasId);
      const ctx = canvas.getContext('2d');
      const w = canvas.width;
      const h = canvas.height;

      // Card background — cream/bone
      ctx.fillStyle = '#f5f0e8';
      ctx.beginPath();
      ctx.roundRect(0, 0, w, h, 6);
      ctx.fill();

      // Subtle inner border
      ctx.strokeStyle = 'rgba(180, 170, 155, 0.3)';
      ctx.lineWidth = 1;
      ctx.beginPath();
      ctx.roundRect(1, 1, w - 2, h - 2, 5);
      ctx.stroke();

      ctx.textAlign = 'center';
      ctx.textBaseline = 'middle';

      // Name — large, letterspaced
      ctx.fillStyle = '#2c2824';
      ctx.font = '500 28px "Cormorant Garamond", serif';
      ctx.letterSpacing = '8px';
      ctx.shadowColor = 'rgba(255,255,255,0.6)';
      ctx.shadowOffsetX = 0;
      ctx.shadowOffsetY = 1;
      ctx.shadowBlur = 0;
      ctx.fillText(data.name, w / 2, h * 0.30);

      // Reset shadow for remaining text
      ctx.shadowColor = 'rgba(255,255,255,0.4)';

      // Title
      ctx.fillStyle = '#4a4540';
      ctx.font = '300 16px "Cormorant Garamond", serif';
      ctx.letterSpacing = '4px';
      ctx.fillText(data.title, w / 2, h * 0.44);

      // Company
      ctx.fillStyle = '#4a4540';
      ctx.font = '400 15px "Cormorant Garamond", serif';
      ctx.letterSpacing = '3px';
      ctx.fillText(data.company, w / 2, h * 0.56);

      // Thin rule
      ctx.strokeStyle = 'rgba(180, 170, 155, 0.4)';
      ctx.lineWidth = 0.5;
      ctx.beginPath();
      ctx.moveTo(w * 0.35, h * 0.66);
      ctx.lineTo(w * 0.65, h * 0.66);
      ctx.stroke();

      // Email
      ctx.fillStyle = '#5a5550';
      ctx.font = '300 14px "Cormorant Garamond", serif';
      ctx.letterSpacing = '2px';
      ctx.fillText(data.email, w / 2, h * 0.77);
    }

    document.fonts.ready.then(() => {
      CARDS.forEach(card => drawCard(card.id, card));

      // Set initial positions
      document.getElementById('card1wrapper').classList.add('front');
      document.getElementById('card2wrapper').classList.add('back');
    });
```

- [ ] **Step 2: Open in browser and verify**

Run: `open index.html`
Expected: Two cream-colored business cards with "BRIAN RICHTER" name, title, company, thin rule, and email rendered in elegant serif typography. Card 1 (Aviary Tech) is in front, Card 2 (Codex / Defined) is behind with a slight offset. Text has a subtle embossed/letterpress look from the white text shadow.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: render business card text on canvas with letterpress effect"
```

---

### Task 4: Click-to-Swap Interaction

**Files:**
- Modify: `index.html` — add swap logic to the script block

- [ ] **Step 1: Add swap handler**

Add after the `document.fonts.ready` block:

```javascript
    let card1Front = true;

    document.getElementById('cardStack').addEventListener('click', (e) => {
      // Don't swap if clicking the email link
      if (e.target.closest('.email-link')) return;

      const wrapper1 = document.getElementById('card1wrapper');
      const wrapper2 = document.getElementById('card2wrapper');

      card1Front = !card1Front;

      if (card1Front) {
        wrapper1.classList.replace('back', 'front');
        wrapper2.classList.replace('front', 'back');
        wrapper1.style.zIndex = '2';
        wrapper2.style.zIndex = '1';
      } else {
        wrapper1.classList.replace('front', 'back');
        wrapper2.classList.replace('back', 'front');
        wrapper1.style.zIndex = '1';
        wrapper2.style.zIndex = '2';
      }
    });
```

- [ ] **Step 2: Set initial z-index values**

Inside the `document.fonts.ready` callback, after the classList lines, add:

```javascript
      document.getElementById('card1wrapper').style.zIndex = '2';
      document.getElementById('card2wrapper').style.zIndex = '1';
```

- [ ] **Step 3: Open in browser and verify**

Run: `open index.html`
Expected: Clicking anywhere on the card stack swaps which card is in front. The transition is smooth (0.5s ease). The back card slides to the offset position, the front card comes forward. Clicking the email area does NOT trigger a swap — it opens the mailto link instead.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add click-to-swap card interaction with smooth transition"
```

---

### Task 5: Polish and Final Touches

**Files:**
- Modify: `index.html` — responsive email link positioning, hover feedback, meta tags

- [ ] **Step 1: Add hover effect on card stack**

Add to the `<style>` block:

```css
    .card-wrapper.front:hover {
      box-shadow: 0 6px 20px rgba(0,0,0,0.45), 0 3px 8px rgba(0,0,0,0.35);
    }

    .email-link:hover {
      background: rgba(0,0,0,0.03);
      border-radius: 2px;
    }
```

- [ ] **Step 2: Add responsive email link sizing**

Add inside the `@media (max-width: 760px)` block:

```css
      .email-link {
        bottom: calc(90vw * 400 / 700 * 0.15);
        width: calc(90vw * 0.37);
        height: calc(90vw * 400 / 700 * 0.08);
      }
```

- [ ] **Step 3: Add meta description (generic, no personal info for bots)**

Add to `<head>`:

```html
  <meta name="description" content="Personal website">
  <meta name="robots" content="noindex, nofollow">
```

- [ ] **Step 4: Open in browser, test on mobile viewport**

Run: `open index.html`
Test in browser dev tools at 375px width (iPhone) and 768px (iPad).
Expected: Cards scale down proportionally. Email link remains clickable. Click/tap to swap still works. Text remains crisp and readable at smaller sizes.

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add hover effects, responsive polish, and meta tags"
```

---

### Task 6: Final Review

- [ ] **Step 1: Verify anti-bot measures**

View page source in browser. Confirm:
- No name, title, company, or email text appears in the HTML source
- Only `<canvas>` elements and `mailto:` hrefs are present
- `user-select: none` is applied via the body style

- [ ] **Step 2: Verify all interactions**

Test:
- Click card stack → cards swap with smooth animation
- Click email area → mailto link opens (no swap triggered)
- Resize window → cards scale proportionally
- Page load → fonts load before canvas renders (no flash of wrong font)

- [ ] **Step 3: Final commit if any adjustments were made**

```bash
git add index.html
git commit -m "polish: final adjustments to business card website"
```
