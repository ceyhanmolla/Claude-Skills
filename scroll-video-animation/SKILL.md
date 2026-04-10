---
name: scroll-video-animation
description: |A comprehensive guide for building high-performance, scroll-triggered video websites. Follow these rules to ensure smooth playback, clean code, and premium results.
---

# Scroll Video Animation Skill

You are an expert in creating high-performance scroll-driven video animations. Generate complete, production-ready implementations that follow best practices for smooth playback, clean code, and premium results.

## When to Use This Skill

Invoke this skill when the user requests:
- Scroll-controlled video playback
- Cinematic narrative experiences with video
- Fullscreen video backgrounds triggered by scroll
- Text animations synchronized with video playback
- High-performance scroll-linked animations

**Do NOT use** for: simple CSS animations, regular video elements with controls, scroll effects without video, or GIF-based animations.

## Core Architecture

### 1. Single HTML File Structure
- Always create a **single self-contained HTML file** with inline CSS and JavaScript
- No external frameworks or build tools
- Structure consists of:
  - Fixed fullscreen `<video>` element at `z-index: 0`
  - Scrollable content overlay with `position: relative; z-index: 1;`
  - Body height set to **5-6× viewport height** for proper scroll range

### 2. Video Element Requirements
```html
<video
  id="scrollVideo"
  muted
  playsinline
  preload="auto"
  style="object-fit: cover; width: 100vw; height: 100vh;">
  <source src="video.mp4" type="video/mp4">
</video>
```

**Critical attributes:**
- `muted` (required for autoplay policies)
- `playsinline` (iOS Safari requirement)
- `preload="auto"` (ensure video is ready)
- `object-fit: cover` (fill viewport)
- **NEVER** use `autoplay` or `controls` - scroll controls playback

**Video specifications:**
- Format: MP4 (best cross-browser support)
- Resolution: 1920×1080 (16:9)
- Duration: **8-15 seconds** (optimal scroll feel)

### 3. Scroll-to-Video Mapping

This is the core mechanic. Map scroll position directly to video `currentTime`:

```javascript
const video = document.getElementById('scrollVideo');

video.addEventListener('loadedmetadata', () => {
  const scrollHeight = document.body.scrollHeight - window.innerHeight;

  let ticking = false;
  window.addEventListener('scroll', () => {
    if (!ticking) {
      requestAnimationFrame(() => {
        const scrollPosition = window.scrollY;
        const scrollFraction = scrollPosition / scrollHeight;
        const videoTime = scrollFraction * video.duration;

        video.currentTime = videoTime;
        ticking = false;
      });
      ticking = true;
    }
  });
});
```

**Key points:**
- Use `requestAnimationFrame` to avoid jank
- Wait for `loadedmetadata` event before attaching scroll listener
- Calculate `scrollFraction` once per frame
- **Never call** `video.play()` - scroll sets `currentTime` directly

### 4. Text Blocks & Scroll Triggers

Text blocks fade in/out based on scroll position. Each has a start and end point:

**HTML Structure:**
```html
<div class="text-block left" data-start="0.15" data-end="0.30">
  <h2>Headline</h2>
  <p>Subtitle or description</p>
</div>
```

**Positioning:**
- Use `position: fixed; z-index: 1;`
- Offset class: `.left`, `.right`, or `.center`
- Padding: `8-12%` from edges
- **Never center** on main subject - always offset

**CSS Base:**
```css
.text-block {
  position: fixed;
  opacity: 0;
  transition: none; /* No CSS transitions - control via JS */
  will-change: transform, opacity;
  padding: 0 8%;
  bottom: 15%;
}

.text-block.left {
  left: 0;
  text-align: left;
}

.text-block.right {
  right: 0;
  text-align: right;
}

.text-block.center {
  left: 50%;
  transform: translateX(-50%);
  text-align: center;
}
```

**JavaScript Animation:**
```javascript
const textBlocks = document.querySelectorAll('.text-block');

window.addEventListener('scroll', () => {
  const scrollFraction = window.scrollY / (document.body.scrollHeight - window.innerHeight);

  textBlocks.forEach(block => {
    const start = parseFloat(block.dataset.start);
    const end = parseFloat(block.dataset.end);

    if (scrollFraction >= start && scrollFraction <= end) {
      const progress = (scrollFraction - start) / (end - start);
      // Smooth fade curve: 0-0.3 fade in, 0.3-0.7 full, 0.7-1 fade out
      let opacity;
      if (progress < 0.3) {
        opacity = progress / 0.3;
      } else if (progress > 0.7) {
        opacity = (1 - progress) / 0.3;
      } else {
        opacity = 1;
      }
      block.style.opacity = opacity;
      // Subtle vertical motion
      const translateY = (1 - Math.min(progress / 0.3, 1)) * 30;
      block.style.transform = `translateY(${translateY}px)`;
    } else {
      block.style.opacity = 0;
    }
  });
});
```

### 5. Navigation Bar (Optional)

Fixed transparent navigation:
```css
nav {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 10;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 40px;
  pointer-events: none; /* Allow clicks through if needed */
}

nav a, nav button {
  pointer-events: auto; /* Re-enable for interactive elements */
}
```

### 6. Performance Optimization Checklist

**Must implement:**
- ✅ `will-change: transform, opacity` on animated elements
- ✅ `requestAnimationFrame` for scroll handler
- ✅ Single `scrollY` read per frame (avoid layout thrashing)
- ✅ `preload="auto"` with loading indicator
- ✅ GPU-accelerated properties only (transform, opacity)

**Loading indicator pattern:**
```javascript
video.addEventListener('canplaythrough', () => {
  const loader = document.getElementById('loader');
  if (loader) loader.style.display = 'none';
});
```

### 7. Mobile Considerations

- **iOS Safari**: `playsinline` attribute is mandatory
- Reduce text size on mobile: use media queries
- Increase padding from edges on mobile
- Add `-webkit-overflow-scrolling: touch;` for smooth momentum
- Test 1:1 scroll feel on touch devices

```css
@media (max-width: 768px) {
  .text-block {
    font-size: 0.9em;
    padding: 0 10%;
  }
}
```

### 8. Complete Reference Template

Always start with this structure:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Scroll Video</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { height: 600vh; font-family: system-ui, sans-serif; background: #000; color: #fff; }
    #scrollVideo {
      position: fixed;
      top: 0; left: 0;
      width: 100vw; height: 100vh;
      object-fit: cover;
      z-index: 0;
    }
    /* Add text-block styles from above */
    /* Add nav styles if needed */
  </style>
</head>
<body>
  <video id="scrollVideo" muted playsinline preload="auto">
    <source src="video.mp4" type="video/mp4">
  </video>

  <!-- Navigation -->
  <!-- Text blocks here -->

  <!-- Loading screen -->
  <div id="loader" style="position:fixed;inset:0;background:#000;display:flex;align-items:center;justify-content:center;z-index:9999;">
    <p>Loading...</p>
  </div>

  <script>
    // All JavaScript from sections 3 & 4 above
    // Add loading indicator removal
  </script>
</body>
</html>
```

## Output Format

Always produce a **complete, ready-to-use HTML file** with:
- Inline CSS in `<style>` tags
- Inline JavaScript in `<script>` tags
- No external dependencies
- Comments explaining key sections
- Proper DOCTYPE and meta tags
- Placeholder for video file (user must add their own MP4)

## Common Pitfalls to Avoid

1. ❌ Using `autoplay` attribute - scroll should control playback
2. ❌ Forgetting `playsinline` - breaks iOS
3. ❌ Using CSS transitions for opacity - implement fade in JS
4. ❌ Reading `scrollY` multiple times per frame - cache it
5. ❌ Using `position: absolute` for text blocks - must be `fixed`
6. ❌ Not adding `will-change` - causes jank on mobile
7. ❌ Making text too short (video under 8s) or too long (over 15s)
8. ❌ Centering text over main video subject - always offset
9. ❌ Not handling `loadedmetadata` - may crash if video not ready
10. ❌ Using layout-thrashing properties - stick to transform/opacity

## User Configuration Options

When user specifies custom parameters, adjust accordingly:

**Scroll height:** Set `body { height: ${multiplier}00vh; }` where multiplier is 5-6 by default
**Text positioning:** Use `.left`, `.right`, or `.center` classes
**Video attributes:** Allow `muted`, `playsinline`, `preload`, `loop` overrides (but never `autoplay`)
**Multiple text blocks:** Each needs unique `data-start`/`data-end` values between 0 and 1

## Example Use Cases

1. **Product showcase**: Hero text fades in as product reveals through video
2. **Narrative story**: Paragraphs appear sequentially as user scrolls
3. **Feature tour**: Text blocks on alternating sides explaining different features
4. **Brand film**: Cinematic experience with minimal text overlays

---

Remember: The goal is **smooth 60fps performance** with cinematic storytelling. Test on mobile devices and ensure 1:1 scroll feel.