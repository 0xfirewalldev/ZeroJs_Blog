---
title: "How I Built My Portfolio"
description: "How I designed my developer portfolio around a system boot sequence, connecting full-stack development with ethical security testing."
pubDate: 'Oct 11 2026'
author: Irikefe M. Kesiena
tags: [astro, tailwindcss, framer-motion, security, portfolio]
canonical_url: https://irikefekesiena.vercel.app
---

# I Built My Portfolio Like a System Boot Sequence: Astro, Tailwind v4, and Framer Motion

Most developer portfolios are a hero, a project grid, and a contact form. Mine had to say something different: I build full-stack apps, then break them the way an attacker would. So the site is structured like a system you're booting into, not a brochure.

Live: https://irikefekesiena.vercel.app
Source: https://github.com/0xfirewalldev/MyPortfolio

## The concept: Build ↔ Break

My work splits in two. I ship Next.js, Prisma and Supabase apps, and I do ethical penetration testing. The portfolio makes that split the information architecture:

- **Boot sequence intro:** `SYS_BOOT // INITIALIZING`, a loading counter, then `STATUS: READY`.
- **Terminal hero card:** `whoami`, `stack --list` and `audit ./app --owasp` run as a tiny shell session instead of a bio paragraph.
- **System Map:** a worktree-style tree with `website` at the root, splitting into **Build** (frontend, backend, data) and **Break** (client-side, server-side, data exposure).

The tree is the part I like most. It shows both skill sets as one mental model: every layer I build has a matching class of bugs I know how to test for. Next.js and server actions pair with broken access control and IDOR. Supabase pairs with RLS bypass.

`[SNIPPET: the tree component / data structure driving Build and Break]`

## Stack choices

**Astro + Tailwind v4.** A portfolio is mostly static content, so shipping zero JS by default is the right starting point. Astro gives me islands only where I need interactivity. Tailwind v4's CSS-first config kept my design tokens (the terminal palette, mono type scale) in one place. I used `astro-icon` for icons without pulling in an icon runtime.

**Framer Motion for motion.** Astro ships no JS by default, so animation lives in React islands. Framer Motion is already my default in React, so the boot sequence and scroll reveals are islands built with `motion` components and `whileInView`, hydrated with `client:load` or `client:visible` only where needed. That keeps the JS cost scoped to the parts that actually animate.

`[SNIPPET: boot sequence timeline]`

## Details that took the most iteration

- **Scroll reveals with hide-on-exit.** Elements animate in and reset when they leave the viewport (`whileInView` with `viewport={{ once: false }}`), so scrolling back up replays the motion. Needs care around `prefers-reduced-motion`; `useReducedMotion` handles it.
- **macOS-style dock navigation.** Compact, always reachable, and it fits the OS metaphor.
- **Tech stack marquee with tap-to-reveal labels.** Hover labels don't exist on touch, so labels are revealed on tap.
- **Horizontal project cards.** Live projects (Vertex Canvas, Next Hub Ecommerce) get a wide layout that fits screenshots and metadata better than a square grid.
- **Bento-grid About section.** Dense but scannable.

`[SNIPPET or screenshot: marquee tap-to-reveal logic]`

## Accessibility: the stylised UI can't cost usability

Terminal aesthetics tend to mean low-contrast green on black and motion everywhere. I ran an accessibility audit and fixed:

- Contrast ratios on muted terminal text
- ARIA labels on icon-only controls (dock, back-to-top)
- Reduced-motion fallbacks for the boot sequence and reveals
- Keyboard focus visibility on custom nav

An offensive-security portfolio with an inaccessible UI would undercut the point.

## Security of the portfolio itself

Static site, small attack surface, but not zero:

- Security headers and CSP set at the Vercel layer. `[confirm and list yours]`
- Contact form: server-side validation, rate limiting, no reflected input. `[confirm]`
- No secrets in the client bundle; env vars scoped properly.

`[SNIPPET: vercel.json headers / CSP]`

## What I'd do differently

- Define the design tokens before building components. I retrofitted contrast fixes I could have avoided.
- Budget animation per page and measure Lighthouse after each addition.
- Write the Build/Break tree data first, then the UI.

## Next

I'm going deeper into practical pentesting (web app attack surface and auth flows), and the blog will cover what I find. If you're building something similar, the repo is open. Feedback welcome.
