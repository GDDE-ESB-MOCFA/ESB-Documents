# MCFA E-Services – Public Portal Installation Guide
Version 1.0.0

## Document Version History

| Version | Release Date | Author | Description |
|-------|-------------|--------|-------------|
| **v1.0.0** | January 2026 | ESB Team | Initial release of Generic Template Public Portal. |
------------------------------------------------------------------------

## 📋 Table of Contents
-   [Technology Stack](#technology-stack)
-   [Prerequisites](#prerequisites)
-   [Installation](#installation)
-   [Project Structure](#project-structure)
-   [Development](#development)
    -   [Start Development Server](#start-development-server)
    -   [Code Quality](#code-quality)
-   [Building for Production](#building-for-production)
-   [Deployment](#deployment)
    -   [Docker](#docker-deployment)

------------------------------------------------------------------------

## Technology Stack

```
Framework: Nuxt 3 (Vue.js)
UI Framework: Nuxt UI v3
Styling: Tailwind CSS
State Management: Pinia
Internationalization: @nuxtjs/i18n
Maps: Open street map & Leaflet
Validation: VeeValidate with Zod
Error Monitoring: Sentry
Build Tool: Vite
```

## Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js**: Version 18 or higher (recommended: 22)
- **Yarn**: Package manager (preferred, as specified in package.json)
- **Git**: For version control

## Installation

1. **Clone the repository:**

   ```bash
   git clone https://github.com/GDDE-ESB-MOCFA/ESB-Registry-Public-Portal
   ```

2. **Install dependencies:**

   ```bash
   # Using yarn (recommended)
   yarn

   # Or using npm
   npm install

   # Or using pnpm
   pnpm install
   ```

3. **Environment Setup:**
   Create a `.env` file in the root directory with the following variables:
   ```env
   NUXT_API_URL=YOUR_API_SERVICE_URL
   NUXT_API_VERSION=YOUR_API_VERSION
   NUXT_PUBLIC_SECRET_KEY_ENCRYPTION=your_secret_key
   NUXT_PUBLIC_APP_DOMAIN=localhost
   APP_MODE=development
   ```

## Project Structure

```
├── assets/                 # Static assets (CSS, images, icons)
├── components/             # Vue components
├── composables/            # Vue composables
├── constants/              # Application constants
├── layouts/                # Page layouts
├── locales/                # Internationalization files
├── middleware/             # Nuxt middleware
├── pages/                  # Application pages/routes
├── plugins/                # Nuxt plugins
├── public/                 # Public static files
├── repositories/           # Data access layer
├── schema/                 # Data validation schemas
├── server/                 # Server-side code
├── services/               # Business logic services
├── stores/                 # Pinia state stores
├── types/                  # TypeScript type definitions
├── utils/                  # Utility functions
├── app.vue                 # Root component
├── nuxt.config.ts          # Nuxt configuration
├── package.json            # Dependencies and scripts
├── tsconfig.json           # TypeScript configuration
├── Dockerfile              # Docker configuration
└── docker-entrypoint.sh    # Docker entrypoint script
```

## Development

### Start Development Server

Run the development server on `http://localhost:3000`:

```bash
# Using yarn
yarn local

# Using npm
npm run local

# Using pnpm
pnpm local
```


### Code Quality

```bash
# Lint code
yarn lint

# Fix linting issues
yarn lint:fix

# Check code formatting
yarn format:check

# Format code
yarn format:write

# Run all pre-deployment checks
yarn prep
```

## Building for Production

1. **Build the application:**

   ```bash
   yarn build
   ```

2. **Preview the production build:**

   ```bash
   yarn preview
   ```

3. **Generate static files (if needed):**
   ```bash
   yarn generate
   ```

------------------------------------------------------------------------
## Deployment

### Docker Deployment

``` bash
#If using window, please run this before run build
sed -i 's/\r$//' docker-entrypoint.sh #For Window OS only
docker build -t esb-registry-public-portal .
docker run -p 3000:3000 -e APP_MODE=prod --env-file .env esb-registry-public-portal
```