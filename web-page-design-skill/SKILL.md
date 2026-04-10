---
name: Web Page Design Skill
description: A comprehensive skill for creating modern, high-performance web pages with video backgrounds, glassmorphic effects, and cinematic animations.
---

## Core Technologies

### Required Stack
- **React 19+** with TypeScript
- **Vite 6+** for build tooling
- **Tailwind CSS v4** (using @tailwindcss/vite plugin)
- **Animation Libraries** (choose based on needs):
  - `gsap` for menu animations, scroll triggers, and complex timelines
  - `framer-motion` for component animations and transitions
  - `motion` (Framer Motion) for React animations
- **Icons**: `lucide-react` for consistent iconography
- **Video**: `hls.js` for HLS video streaming (optional)

### Font Systems

#### Google Fonts Integration
Always import fonts in `index.css`:

```css
@import url('https://fonts.googleapis.com/css2?family=FontName:ital,wght@0,400;0,700;1,400&display=swap');
```

#### Common Font Patterns

**Display/Heading Fonts:**
- `Instrument Serif` - Elegant serif for headings (italic + regular)
- `Anton` - Bold grotesque for uppercase headings
- `Schibsted Grotesk` - Modern sans-serif
- `Fustat` - Arabic-friendly display font
- `General Sans` - Clean geometric sans-serif

**Body/Paragraph Fonts:**
- `Inter` - Versatile sans-serif (weights 400-700)
- `Geist Sans` - Modern system font alternative
- `Noto Sans` - Multi-language support

**Accent/Script Fonts:**
- `Condiment` - Cursive script for accent text
- `Instrumental Serif` - Display serif

#### Font Variable Setup
```css
:root {
  --font-display: 'Instrument Serif', serif;
  --font-body: 'Inter', sans-serif;
}
```

## Design Patterns

### 1. Video Background System

#### Standard Video Background
```tsx
<video
  autoPlay
  loop
  muted
  playsInline
  className="absolute inset-0 w-full h-full object-cover"
>
  <source src="VIDEO_URL" type="video/mp4" />
</video>
```

#### Seamless Loop with Fade Transitions
```tsx
const videoRef = useRef<HTMLVideoElement>(null);
const [opacity, setOpacity] = useState(0);
const fadingOutRef = useRef(false);

useEffect(() => {
  const video = videoRef.current;
  if (!video) return;

  const fadeIn = () => {
    let currentOpacity = 0;
    const animate = () => {
      currentOpacity += 0.02;
      if (currentOpacity >= 1) {
        setOpacity(1);
        return;
      }
      setOpacity(currentOpacity);
      requestAnimationFrame(animate);
    };
    animate();
  };

  const fadeOut = () => {
    if (fadingOutRef.current) return;
    fadingOutRef.current = true;

    let currentOpacity = opacity;
    const animate = () => {
      currentOpacity -= 0.02;
      if (currentOpacity <= 0) {
        setOpacity(0);
        return;
      }
      setOpacity(currentOpacity);
      requestAnimationFrame(animate);
    };
    animate();
  };

  const handleTimeUpdate = () => {
    const remaining = video.duration - video.currentTime;
    if (remaining <= 0.55 && !fadingOutRef.current) {
      fadeOut();
    }
  };

  const handleEnded = () => {
    setOpacity(0);
    fadingOutRef.current = false;
    setTimeout(() => {
      video.currentTime = 0;
      video.play();
      fadeIn();
    }, 100);
  };

  video.addEventListener('canplay', () => {
    video.play();
    fadeIn();
  });
  video.addEventListener('timeupdate', handleTimeUpdate);
  video.addEventListener('ended', handleEnded);

  return () => {
    video.removeEventListener('canplay', () => video.play());
    video.removeEventListener('timeupdate', handleTimeUpdate);
    video.removeEventListener('ended', handleEnded);
  };
}, [opacity]);

return (
  <video
    ref={videoRef}
    autoPlay
    muted
    playsInline
    style={{ opacity }}
    className="absolute inset-0 w-full h-full object-cover"
  >
    <source src="VIDEO_URL" type="video/mp4" />
  </video>
);
```

#### HLS Video Streaming
```tsx
import Hls from 'hls.js';

const HlsVideo = ({ src }: { src: string }) => {
  const videoRef = useRef<HTMLVideoElement>(null);

  useEffect(() => {
    const video = videoRef.current;
    if (!video) return;

    if (Hls.isSupported()) {
      const hls = new Hls();
      hls.loadSource(src);
      hls.attachMedia(video);
      return () => hls.destroy();
    } else if (video.canPlayType('application/vnd.apple.mpegurl')) {
      video.src = src;
    }
  }, [src]);

  return (
    <video
      ref={videoRef}
      autoPlay
      loop
      muted
      playsInline
      className="absolute inset-0 w-full h-full object-cover"
    />
  );
};
```

### 2. Liquid Glass Effect

#### CSS Implementation
Add to `index.css` in `@layer components`:

```css
.liquid-glass {
  background: rgba(255, 255, 255, 0.01);
  background-blend-mode: luminosity;
  backdrop-filter: blur(4px);
  -webkit-backdrop-filter: blur(4px);
  border: none;
  box-shadow: inset 0 1px 1px rgba(255, 255, 255, 0.1);
  position: relative;
  overflow: hidden;
}

.liquid-glass::before {
  content: '';
  position: absolute;
  inset: 0;
  border-radius: inherit;
  padding: 1.4px;
  background: linear-gradient(180deg,
    rgba(255,255,255,0.45) 0%,
    rgba(255,255,255,0.15) 20%,
    rgba(255,255,255,0) 40%,
    rgba(255,255,255,0) 60%,
    rgba(255,255,255,0.15) 80%,
    rgba(255,255,255,0.45) 100%
  );
  -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
  -webkit-mask-composite: xor;
  mask-composite: exclude;
  pointer-events: none;
}
```

#### Usage Examples
```tsx
// Navigation pill
<nav className="liquid-glass rounded-full px-6 py-3">...</nav>

// Card
<div className="liquid-glass rounded-3xl p-6">...</div>

// Button
<button className="liquid-glass rounded-full px-6 py-3">...</button>
```

### 3. Color System

#### HSL-based Design Tokens
```css
:root {
  /* Dark Theme */
  --background: 260 87% 3%;
  --foreground: 40 6% 95%;
  --surface: 0 0% 8%;
  --muted: 0 0% 53%;
  --stroke: 0 0% 12%;
  --accent: 262 83% 58%;

  /* Light Theme (if needed) */
  --light-background: 0 0% 100%;
  --light-foreground: 0 0% 4%;
}
```

#### Tailwind Config Mapping
```ts
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        background: 'hsl(var(--background))',
        foreground: 'hsl(var(--foreground))',
        surface: 'hsl(var(--surface))',
        muted: 'hsl(var(--muted))',
        stroke: 'hsl(var(--stroke))',
        accent: 'hsl(var(--accent))',
      },
    },
  },
};
```

#### Common Color Palettes

**Deep Space Theme:**
```css
--background: 222 47% 11%; /* #010828 */
--foreground: 220 100% 97%; /* #EFF4FF */
--accent: 84 100% 50%; /* #6FFF00 */
```

**Premium Dark:**
```css
--background: 0 0% 4%;
--foreground: 0 0% 96%;
--surface: 0 0% 8%;
```

**Navy Blue:**
```css
--background: 201 100% 13%;
--foreground: 0 0% 100%;
```

### 4. Animation Patterns

#### GSAP Animations

**Staggered Menu:**
```tsx
import { gsap } from 'gsap';

const StaggeredMenu = ({ isOpen }: { isOpen: boolean }) => {
  const menuRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (!menuRef.current) return;

    if (isOpen) {
      gsap.fromTo(
        menuRef.current.children,
        { yPercent: 140, rotate: 10 },
        { yPercent: 0, rotate: 0, duration: 1, ease: 'power4.out', stagger: 0.1 }
      );
    } else {
      gsap.to(menuRef.current.children, {
        yPercent: 140,
        rotate: 10,
        duration: 0.8,
        ease: 'power3.in',
        stagger: 0.05,
      });
    }
  }, [isOpen]);

  return (
    <div ref={menuRef}>
      {menuItems.map((item) => (
        <div key={item}>{item}</div>
      ))}
    </div>
  );
};
```

**ScrollTrigger Parallax:**
```tsx
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

useEffect(() => {
  gsap.to(parallaxRef.current, {
    yPercent: -50,
    ease: 'none',
    scrollTrigger: {
      trigger: sectionRef.current,
      start: 'top bottom',
      end: 'bottom top',
      scrub: true,
    },
  });
}, []);
```

#### Framer Motion Animations

**Text Reveal:**
```tsx
import { motion } from 'framer-motion';

const textVariants = {
  hidden: { opacity: 0, y: 40 },
  visible: { opacity: 1, y: 0 },
};

<motion.h1
  variants={textVariants}
  initial="hidden"
  animate="visible"
  transition={{ duration: 0.96, ease: [0.22, 1, 0.36, 1] }}
>
  Your Text
</motion.h1>
```

**Staggered Children:**
```tsx
<motion.div
  initial="hidden"
  animate="visible"
  variants={{
    hidden: {},
    visible: {
      transition: {
        staggerChildren: 0.12,
      },
    },
  }}
>
  {items.map((item) => (
    <motion.div
      key={item}
      variants={{
        hidden: { opacity: 0, y: 40 },
        visible: { opacity: 1, y: 0 },
      }}
    >
      {item}
    </motion.div>
  ))}
</motion.div>
```

**Scroll-triggered Animation:**
```tsx
import { useInView } from 'framer-motion';

const ref = useRef(null);
const isInView = useInView(ref, { once: true, margin: '-100px' });

<motion.div
  ref={ref}
  initial={{ opacity: 0, y: 40 }}
  animate={isInView ? { opacity: 1, y: 0 } : {}}
  transition={{ duration: 0.8 }}
>
  Content
</motion.div>
```

#### Custom CSS Animations

```css
@keyframes fade-rise {
  from { opacity: 0; transform: translateY(24px); }
  to { opacity: 1; transform: translateY(0); }
}

.animate-fade-rise {
  animation: fade-rise 0.8s ease-out both;
}

@keyframes marquee {
  0% { transform: translateX(0%); }
  100% { transform: translateX(-50%); }
}

.animate-marquee {
  animation: marquee 20s linear infinite;
}

@keyframes scroll-down {
  0% { transform: translateY(-100%); }
  50% { transform: translateY(200%); }
  100% { transform: translateY(-100%); }
}

.animate-scroll-down {
  animation: scroll-down 1.5s ease-in-out infinite;
}
```

### 5. Component Patterns

#### Navigation Bar
```tsx
const Navbar = () => {
  const [isScrolled, setIsScrolled] = useState(false);

  useEffect(() => {
    const handleScroll = () => setIsScrolled(window.scrollY > 100);
    window.addEventListener('scroll', handleScroll);
    return () => window.removeEventListener('scroll', handleScroll);
  }, []);

  return (
    <nav
      className={`fixed top-0 left-0 right-0 z-50 flex justify-center pt-4 md:pt-6 px-4 ${
        isScrolled ? 'shadow-md shadow-black/10' : ''
      }`}
    >
      <div className="inline-flex items-center rounded-full backdrop-blur-md border border-white/10 bg-surface px-2 py-2">
        {/* Logo */}
        <div className="w-9 h-9 rounded-full border-2 border-accent-gradient flex items-center justify-center">
          <span className="font-display italic text-sm">Logo</span>
        </div>

        {/* Nav Links */}
        {['Home', 'Work', 'About'].map((item) => (
          <button
            key={item}
            className="text-xs sm:text-sm rounded-full px-3 sm:px-4 py-1.5 sm:py-2 text-muted hover:text-foreground hover:bg-stroke/50 transition-colors"
          >
            {item}
          </button>
        ))}

        {/* CTA */}
        <button className="text-xs sm:text-sm rounded-full px-3 sm:px-4 py-1.5 sm:py-2 text-foreground bg-stroke/50 hover:bg-stroke transition-colors">
          Contact
        </button>
      </div>
    </nav>
  );
};
```

#### Hero Section
```tsx
const HeroSection = () => {
  return (
    <section className="relative h-screen overflow-hidden">
      {/* Video Background */}
      <VideoBackground src="VIDEO_URL" />

      {/* Content */}
      <div className="relative z-10 flex flex-col items-center justify-center h-full px-6">
        {/* Eyebrow */}
        <p className="text-xs text-muted uppercase tracking-[0.3em] mb-8">
          COLLECTION '26
        </p>

        {/* Heading */}
        <h1 className="text-6xl md:text-8xl lg:text-9xl font-display italic leading-[0.9] tracking-tight text-foreground mb-6">
          Your Heading
        </h1>

        {/* Subtext */}
        <p className="text-sm md:text-base text-muted max-w-md mb-12">
          Your description text goes here.
        </p>

        {/* CTA Buttons */}
        <div className="flex gap-4">
          <button className="rounded-full text-sm px-7 py-3.5 bg-foreground text-bg hover:scale-105 transition-transform">
            Primary Action
          </button>
          <button className="rounded-full text-sm px-7 py-3.5 border-2 border-stroke bg-bg text-foreground hover:scale-105 transition-transform">
            Secondary Action
          </button>
        </div>
      </div>

      {/* Scroll Indicator */}
      <div className="absolute bottom-8 left-1/2 -translate-x-1/2 flex flex-col items-center gap-2">
        <span className="text-xs text-muted uppercase tracking-[0.2em]">SCROLL</span>
        <div className="w-px h-10 bg-stroke animate-scroll-down" />
      </div>
    </section>
  );
};
```

#### Card Grid
```tsx
const CardGrid = ({ items }: { items: CardItem[] }) => {
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {items.map((item, index) => (
        <motion.div
          key={item.id}
          initial={{ opacity: 0, y: 50 }}
          whileInView={{ opacity: 1, y: 0 }}
          viewport={{ once: true, margin: '-100px' }}
          transition={{ duration: 0.8, delay: index * 0.15 }}
          className="liquid-glass rounded-3xl overflow-hidden group"
        >
          <div className="aspect-video overflow-hidden">
            <img
              src={item.image}
              alt={item.title}
              className="w-full h-full object-cover transition-transform duration-700 group-hover:scale-105"
            />
          </div>
          <div className="p-6">
            <span className="text-xs uppercase tracking-widest text-muted mb-2 block">
              {item.category}
            </span>
            <h3 className="text-xl font-semibold text-foreground mb-2">
              {item.title}
            </h3>
            <p className="text-sm text-muted">{item.description}</p>
          </div>
        </motion.div>
      ))}
    </div>
  );
};
```

#### Marquee
```tsx
const Marquee = ({ items }: { items: string[] }) => {
  return (
    <div className="overflow-hidden">
      <div className="flex animate-marquee">
        {[...items, ...items].map((item, index) => (
          <div key={index} className="flex-shrink-0 px-8">
            {item}
          </div>
        ))}
      </div>
    </div>
  );
};
```

### 6. Responsive Design Patterns

#### Mobile-First Approach
```tsx
// Mobile: stacked, Desktop: side-by-side
<div className="flex flex-col md:flex-row gap-6">
  <div className="flex-1">Left content</div>
  <div className="flex-1">Right content</div>
</div>

// Responsive typography
<h1 className="text-4xl md:text-6xl lg:text-8xl">
  Responsive Heading
</h1>

// Responsive spacing
<div className="p-4 md:p-8 lg:p-12">
  Content
</div>

// Hide on mobile, show on desktop
<div className="hidden md:block">
  Desktop-only content
</div>
```

#### Breakpoint System
- `sm`: 640px
- `md`: 768px
- `lg`: 1024px
- `xl`: 1280px
- `2xl`: 1536px

### 7. Performance Optimization

#### Video Optimization
- Use `playsInline` for mobile
- Set `preload="auto"` for above-the-fold videos
- Consider using HLS for streaming
- Implement proper fade transitions for seamless loops

#### Animation Performance
```tsx
// Use transforms instead of layout properties
<motion.div
  animate={{ x: 100 }} // Good
  // animate={{ left: 100 }} // Bad - causes layout thrashing
/>

// Use will-change sparingly
<div style={{ willChange: 'transform, opacity' }}>

// Batch animations
useEffect(() => {
  const ctx = gsap.context(() => {
    gsap.to('.item', { x: 100, stagger: 0.1 });
  }, containerRef);
  return () => ctx.revert();
}, []);
```

#### Image Optimization
```tsx
// Use next/image or similar optimization
<img
  src={imageSrc}
  alt={altText}
  loading="lazy"
  decoding="async"
  className="object-cover"
/>
```

## Common Page Sections

### 1. Hero Section
- Full viewport height (min-h-screen or h-screen)
- Video or gradient background
- Centered content with eyebrow, heading, subtext, and CTAs
- Scroll indicator at bottom

### 2. About/Intro Section
- Two-column layout on desktop
- Large typography with accent words
- Subtle gradient overlays
- Scroll-triggered animations

### 3. Featured/Showcase Section
- Grid layout (2-3 columns)
- Cards with hover effects
- Video or image backgrounds
- Liquid glass overlays

### 4. Services/Features Section
- Alternating layout (text + visual)
- Icon-based feature lists
- Gradient backgrounds
- Staggered animations

### 5. Testimonials/Social Proof
- Logo marquee
- Quote cards
- Stats grid
- Video testimonials

### 6. CTA Section
- Full-width background
- Centered content
- Large heading
- Prominent CTA button

### 7. Footer
- Multi-column layout
- Social links
- Newsletter signup
- Legal links

## Best Practices

### Typography
- Use consistent font pairings
- Maintain proper line heights (0.85-1.1 for headings, 1.5-1.7 for body)
- Use tracking for uppercase text (tracking-widest or custom values)
- Limit font weights to 2-3 per design

### Color
- Use HSL for theme colors
- Maintain adequate contrast ratios
- Use accent colors sparingly
- Consider dark mode by default for premium feel

### Spacing
- Use consistent spacing scale (4px, 8px, 16px, 24px, 32px, 48px, 64px)
- Max-width containers for readability (max-w-7xl, max-w-6xl)
- Generous padding for premium feel

### Animation
- Use easing curves that feel natural ([0.22, 1, 0.36, 1] for smooth motion)
- Keep durations under 1s for UI animations
- Use stagger for multiple elements
- Respect prefers-reduced-motion

### Accessibility
- Use semantic HTML
- Provide alt text for images
- Ensure keyboard navigation
- Maintain focus states
- Use ARIA labels when needed

## Project Structure

```
src/
├── components/
│   ├── ui/              # Reusable UI components
│   ├── layout/          # Layout components (Navbar, Footer)
│   ├── sections/        # Page sections
│   └── animations/      # Animation utilities
├── hooks/               # Custom hooks
├── styles/
│   ├── index.css        # Global styles
│   └── globals.css      # CSS variables
├── types/               # TypeScript types
├── utils/               # Utility functions
└── App.tsx              # Main app component
```

## Quick Start Template

```tsx
import { useState, useEffect, useRef } from 'react';
import { motion } from 'framer-motion';

const LandingPage = () => {
  return (
    <main className="min-h-screen bg-background text-foreground">
      {/* Hero Section */}
      <section className="relative h-screen overflow-hidden">
        <VideoBackground src="YOUR_VIDEO_URL" />
        <div className="relative z-10 flex flex-col items-center justify-center h-full px-6">
          <motion.h1
            initial={{ opacity: 0, y: 40 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.96, ease: [0.22, 1, 0.36, 1] }}
            className="text-6xl md:text-8xl font-display italic"
          >
            Your Heading
          </motion.h1>
        </div>
      </section>

      {/* Additional Sections */}
      {/* ... */}
    </main>
  );
};
```

## Common Issues & Solutions

### Video Not Playing
- Ensure `autoPlay`, `muted`, and `playsInline` attributes are set
- Check video format (MP4/H.264 recommended)
- Verify video URL is accessible

### Animation Performance Issues
- Use `transform` and `opacity` only
- Avoid animating layout properties
- Use `will-change` sparingly
- Consider reducing animation complexity on mobile

### Glass Effect Not Working
- Ensure backdrop-filter is supported
- Add `-webkit-backdrop-filter` for Safari
- Check z-index stacking context

### Responsive Layout Breaking
- Use mobile-first approach
- Test at all breakpoints
- Use `min-width` instead of `max-width` for progressive enhancement
- Consider container queries for component-level responsiveness

## Resources

### Documentation
- [Tailwind CSS](https://tailwindcss.com/docs)
- [Framer Motion](https://www.framer.com/motion/)
- [GSAP](https://greensock.com/docs/)
- [React](https://react.dev/)

### Design Inspiration
- Awwwards
- SiteInspire
- Dribbble
- Behance

### Video Resources
- Pexels
- Pixabay
- Coverr
- Mixkit

## License

This skill is provided as-is for creating web pages. Ensure you have proper licenses for any assets (videos, images, fonts) used in production.