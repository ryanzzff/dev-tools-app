# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A client-side developer tools web application built with SvelteKit 5 and Tailwind CSS. The application provides essential utilities for developers including Unix timestamp conversion, Base64 encoding/decoding, URL encoding/decoding, JSON formatting, UUID generation, and hash generation. All processing is done entirely client-side with no server communication.

## Development Commands

- `npm run dev` - Start development server (runs on http://localhost:5173)
- `npm run dev -- --open` - Start development server and open in browser
- `npm run build` - Create production build
- `npm run preview` - Preview production build locally
- `npm run check` - Run TypeScript and Svelte checks
- `npm run check:watch` - Run checks in watch mode
- `npm run lint` - Run linting checks (Prettier + ESLint)
- `npm run format` - Format code with Prettier
- `npm run test:unit` - Run unit tests with Vitest
- `npm run test:e2e` - Run end-to-end tests with Playwright
- `npm run test` - Run both unit and e2e tests

## Architecture

### Framework Stack
- **SvelteKit 5** with the new runes syntax (`$state`, `$derived`)
- **Tailwind CSS 4** for styling with forms and typography plugins
- **TypeScript** with strict configuration
- **Vite** as the build tool

### Route Structure
The application uses file-based routing with individual tool pages:

- `/` - Main landing page with tool grid and search functionality
- `/timestamp` - Unix timestamp converter with real-time updates
- `/base64` - Base64 encoding/decoding with error handling
- `/url` - URL encoding/decoding
- `/json` - JSON formatter and validator
- `/uuid` - UUID v4 generator with bulk generation
- `/hash` - Hash generator (SHA-1, SHA-256, SHA-512)

### Key Components
- `+layout.svelte` - Global layout with navigation header and gradient background
- `+page.svelte` - Main landing page with searchable tool grid
- Individual tool pages follow consistent UI patterns with gradient backgrounds, error handling, and real-time processing

### Svelte 5 Patterns
The codebase uses Svelte 5's new runes syntax:
- `$state()` for reactive variables
- `$derived()` for computed values
- Traditional reactive statements (`$:`) are still used where appropriate

### Styling Approach
- Tailwind utility classes with custom gradients and backdrop blur effects
- Consistent design system with purple/indigo/blue color scheme
- Responsive grid layouts (1 column on mobile, 2 on tablet, 3 on desktop)
- Hover animations and transitions throughout

#### Design Features

**Modern Glassmorphism Design**
- Gradient backgrounds with blur effects
- Semi-transparent cards with backdrop blur
- Beautiful shadow and border effects
- Smooth animations and transitions

**Enhanced Visual Elements**
- Gradient Headers - Beautiful color-coded icons and titles
- Better Navigation - Rounded tabs with hover effects
- Improved Typography - Gradient text and better spacing
- Color-Coded Tools - Each tool has its own color theme:
  - 🔵 Timestamp: Blue theme
  - 🟢 Base64: Emerald/Green theme
  - 🟡 URL: Amber/Orange theme
  - 🟣 JSON: Purple theme
  - 🌸 UUID: Pink theme
  - 🔴 Hash: Red theme

**Better User Experience**
- Larger Input Areas - More space for content
- Enhanced Buttons - Gradient effects with smooth hovers
- Better Error Handling - Icons with error messages
- Improved Information Panels - More detailed and styled

**Professional Polish**
- Consistent Spacing - Better margins and padding
- Smooth Animations - 300ms transitions everywhere
- Focus States - Beautiful ring effects on inputs
- Disabled States - Proper visual feedback

### Client-Side Architecture
All tools perform processing entirely in the browser:
- No API calls or server dependencies
- Uses native browser APIs (btoa/atob for Base64, crypto for hashing, etc.)
- Real-time updates without user interaction where appropriate
- Input validation and error handling for all utilities

## Important Notes

- All tools are completely client-side - no data is sent to servers
- Search functionality on main page filters tools by title and description
- Uses Svelte 5 syntax which may require specific handling for reactive declarations
- Tailwind CSS 4 syntax for importing and plugins
- TypeScript strict mode enabled
- Responsive design with mobile-first approach