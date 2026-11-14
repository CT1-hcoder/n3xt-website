# N3XT Technologies — Corporate Website

Slogan: **Dream. Innovate. Transform.**
Positioning: *Engineering the Future of Intelligence.*

Built with Next.js (App Router), TypeScript and Tailwind CSS 3.

## Requirements

- Node.js **20.9 or newer** (a `.nvmrc` file is included: `nvm use`)
- npm
- Internet access on the first run (fonts are downloaded from Google Fonts)

## Run locally

```
npm install
npm run dev
```

Open http://localhost:3000. To test on a phone on the same Wi-Fi, use the
"Network" address printed in the terminal.

## Production build

```
npm run build
npm run start
```

`npm run build` also type-checks the code. Run it before every deployment.

## Environment variables

Copy `.env.example` to `.env.local` and fill in the values. Never commit `.env.local`.

| Variable | Required | Purpose |
|---|---|---|
| `NEXT_PUBLIC_SITE_URL` | Yes (production) | Public address, used for sitemap, share previews and search engines |
| `RESEND_API_KEY` | For the contact form | API key from https://resend.com |
| `CONTACT_TO_EMAIL` | For the contact form | Where messages are delivered |
| `CONTACT_FROM_EMAIL` | For the contact form | Sender address allowed by Resend |

Without the three contact variables the site works, but the contact form shows
"The contact service is not configured yet" instead of sending.

## Project structure

```
app/                  Pages and routes
  layout.tsx          Page frame, fonts, SEO metadata, structured data
  page.tsx            Homepage
  about/ services/ products/ research/ careers/ contact/
  api/contact/        Contact form backend (sends email through Resend)
  sitemap.ts robots.ts not-found.tsx error.tsx icon.svg
components/           Reusable UI (Header, Footer, Logo, Section, PageHeader,
                      Reveal, EmptyState, ContactForm; PageStub is unused)
lib/                  All editable content and shared code (see below)
public/brand/         Logo files (SVG)
public/og-image.png   Social share image (1200x630)
docs/                 Deployment guide, brand guidelines, og-image source
```

## Editing content

All text lives in `lib/`. You rarely need to touch the pages.

| To change | Edit |
|---|---|
| Company name, slogan, navigation | `lib/site.ts` |
| Homepage text, mission, vision | `lib/content.ts` |
| About page: story, values, CEO profile and photo | `lib/about.ts` (photo in `public/team/`) |
| Service descriptions | `lib/services.ts` |
| Public products and projects | `lib/products.ts` |
| Research areas | `lib/research.ts` |
| Job openings, careers text | `lib/careers.ts` |
| Email, phone, location, social links, form topics | `lib/contact.ts` |
| Colours and fonts | `tailwind.config.ts`, `app/layout.tsx` |

Confidential internal projects must never be added to `lib/products.ts`; that
file is shown publicly.

## Placeholders still to fill in

- Company location (`lib/contact.ts`)
- More social links, for example Instagram and Discord (`lib/contact.ts`)
- More leadership profiles, if wanted (`ceo` in `lib/about.ts`; copy its pattern)
- Further products and projects (`lib/products.ts`)
- Job openings (`lib/careers.ts`)

Done: the company story, mission, vision, philosophy and the CEO profile with photo
(`lib/about.ts`, `lib/content.ts`, `public/team/`), and Yasmine AI as the first product.

Wording marked DRAFT (research areas, careers text, service bullets, the short
line under each core value) should be reviewed and confirmed by the company.

Social logos: `components/SocialIcon.tsx` (TikTok, X, Instagram, Discord, YouTube,
Facebook and GitHub from Simple Icons, CC0; LinkedIn from Font Awesome Free, CC BY 4.0).
Keep the licence comment at the top of that file.

## Brand

Logos are in `public/brand/`. Colours, typography and usage rules are in
`docs/BRAND-GUIDELINES.md`.

## Deployment

See `docs/DEPLOYMENT.md`.

## Known limitations

- The contact form rate limit is kept in server memory. It slows casual abuse
  but resets on restart and is per server instance.
- The form cannot upload files (such as a CV). Applicants send a link instead.
- English only. No analytics, no CMS, no automated test suite.
- The author could not run the site themselves. It has been checked by the
  owner in the development server. A production build, a Lighthouse audit and
  a real contact-form email test still need to be done on your side.

## Animation, music and social links

- **Animation:** defined in `tailwind.config.ts` (keyframes) and used in
  `app/page.tsx` (hero), `components/Reveal.tsx` (fade-in on scroll),
  `components/HeroEmblem.tsx`, `components/ScrollProgress.tsx` and
  `app/template.tsx` (page fade). Visitors whose device asks for reduced motion
  get a still page automatically (`app/globals.css`).
- **The five effects and how to use them anywhere:**
  - Fade up, scale pop, slide in: wrap content in
    `<Reveal variant="fade-up | pop | slide-left | slide-right | fade" delay={100}>`
    (`components/Reveal.tsx`). It plays once when the content scrolls into view.
  - Typewriter: `<h1><Typewriter text="Your words" /></h1>` (`components/Typewriter.tsx`).
    Used on the homepage headline.
  - 3D tilt on hover: use `<Tilt className="...card look...">` instead of a card `<div>`
    (`components/Tilt.tsx`). It follows the mouse only, never touch.
- **Background music:** `components/MusicToggle.tsx` plays
  `public/audio/n3xt-ambient.mp3` in a loop. It is OFF until the visitor presses the
  Music button (browsers do not allow sound to start by itself), and the choice is
  remembered. The track is original and was generated by `docs/generate-ambient.py`
  (no licence needed). To use a different track, replace the mp3 file with the same
  name and keep it small (under 2 MB).
- **Social links:** edit the `socials` list in `lib/contact.ts`. Ready-to-use example
  lines for Instagram, Discord, YouTube, Facebook and GitHub are in that file.
