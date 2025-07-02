# 🛠️ Developer Tools

A beautiful collection of essential utilities for modern developers. Fast, secure, and completely client-side.

![Developer Tools](https://img.shields.io/badge/Built%20with-SvelteKit%205-FF3E00?style=for-the-badge&logo=svelte&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=for-the-badge&logo=typescript&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)

![screenshots](../docs/images/SCR-20250702-lmnf.png)

## ✨ Features

### 🔧 Available Tools

- **🔵 Unix Timestamp** - Convert between Unix timestamps and human-readable dates with real-time updates
- **🟢 Base64 Encode/Decode** - Encode and decode Base64 strings with instant conversion and error handling
- **🟡 URL Encode/Decode** - Encode and decode URL components with proper percent encoding
- **🟣 JSON Formatter** - Format, validate, and minify JSON data with syntax highlighting
- **🌸 UUID Generator** - Generate cryptographically secure UUIDs (v4) in bulk with one click
- **🔴 Hash Generator** - Generate secure hashes using SHA-1, SHA-256, and SHA-512 algorithms

### 🎨 Design Features

**Modern Glassmorphism Design**
- Gradient backgrounds with blur effects
- Semi-transparent cards with backdrop blur
- Beautiful shadow and border effects
- Smooth animations and transitions

**Enhanced User Experience**
- 🔍 **Smart Search** - Instantly filter tools as you type
- 🎯 **Real-time Processing** - See results as you work
- 🛡️ **100% Client-side** - No data sent to servers
- 📱 **Responsive Design** - Works perfectly on all devices
- ⚡ **Lightning Fast** - No server round-trips needed

## 🚀 Quick Start

### Prerequisites

- Node.js 18+ installed
- npm, pnpm, or yarn package manager

### Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd dev-tools-app
```

2. Install dependencies:
```bash
npm install
# or
pnpm install
# or
yarn install
```

3. Start the development server:
```bash
npm run dev
# or
npm run dev -- --open  # Opens in browser automatically
```

4. Open your browser and navigate to `http://localhost:5173`

## 🛠️ Development

### Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Create production build
- `npm run preview` - Preview production build locally
- `npm run check` - Run TypeScript and Svelte checks
- `npm run lint` - Run linting checks (Prettier + ESLint)
- `npm run format` - Format code with Prettier
- `npm run test:unit` - Run unit tests with Vitest
- `npm run test:e2e` - Run end-to-end tests with Playwright
- `npm run test` - Run both unit and e2e tests

### Tech Stack

- **Framework**: SvelteKit 5 with new runes syntax
- **Styling**: Tailwind CSS 4 with forms and typography plugins
- **Language**: TypeScript with strict configuration
- **Build Tool**: Vite
- **Testing**: Vitest + Playwright
- **Linting**: ESLint + Prettier

## 📁 Project Structure

```
src/
├── routes/
│   ├── +layout.svelte          # Global layout with navigation
│   ├── +page.svelte            # Main landing page with search
│   ├── timestamp/+page.svelte  # Unix timestamp converter
│   ├── base64/+page.svelte     # Base64 encoder/decoder
│   ├── url/+page.svelte        # URL encoder/decoder
│   ├── json/+page.svelte       # JSON formatter
│   ├── uuid/+page.svelte       # UUID generator
│   └── hash/+page.svelte       # Hash generator
├── app.css                     # Global styles (Tailwind imports)
└── app.html                    # HTML template
```

## 🎯 Key Features

### Security & Privacy
- **100% Client-side Processing** - All data stays in your browser
- **No Server Communication** - No data transmission to external servers
- **No Data Storage** - Nothing is saved or tracked

### Performance
- **Real-time Updates** - Instant feedback as you type
- **Lightweight** - Minimal bundle size with efficient code splitting
- **Fast Load Times** - Optimized for performance

### Accessibility
- **Keyboard Navigation** - Full keyboard support
- **Screen Reader Friendly** - Proper ARIA labels and semantic HTML
- **High Contrast** - Meets WCAG guidelines

## 🧪 Testing

Run the test suite:

```bash
# Unit tests
npm run test:unit

# End-to-end tests
npm run test:e2e

# All tests
npm run test
```

## 🚀 Deployment

Build for production:

```bash
npm run build
```

The built application will be in the `build/` directory, ready for deployment to any static hosting service.

### Deployment Options

- **Vercel**: Automatic deployment with SvelteKit adapter
- **Netlify**: Static site hosting
- **GitHub Pages**: Free hosting for public repositories
- **Any Static Host**: The build output is standard HTML/CSS/JS

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Built with [SvelteKit](https://kit.svelte.dev/)
- Styled with [Tailwind CSS](https://tailwindcss.com/)
- Icons from [Heroicons](https://heroicons.com/)
- Deployed with [Vercel](https://vercel.com/)

---

<div align="center">
  <p>Made with ❤️ for developers, by developers</p>
  <p>
    <a href="#top">Back to top</a>
  </p>
</div>