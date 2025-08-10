# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Essential Commands
- `npm run dev` - Start development server with hot reload
- `npm run build` - Build production application
- `npm run preview` - Preview production build locally

### Testing & Quality
- `npm run test` - Run tests in watch mode (Vitest)
- `npm run test_run` - Run tests once
- `npm run check` - TypeScript and Svelte type checking
- `npm run check:watch` - Type checking in watch mode
- `npm run lint` - ESLint code linting
- `npm run format` - Format code with Prettier
- `npm run format_check` - Check code formatting
- `./checks.sh` - Run all quality checks (format, lint, typecheck, tests)

### Single Test Execution
Use Vitest's built-in filtering: `npm run test -- <test-file-name>` or `npm run test_run -- <test-file-name>`

## Architecture Overview

### Tech Stack
- **Framework**: SvelteKit 2.x with TypeScript
- **Styling**: TailwindCSS + DaisyUI component library
- **Database**: Supabase (PostgreSQL) with typed client
- **Authentication**: Supabase Auth with OAuth (GitHub) support
- **Email**: Resend with Handlebars templates
- **Search**: Client-side with Fuse.js indexing prerendered content

### Project Structure

**Route Groups Architecture**:
- `(marketing)/` - Public pages (homepage, blog, auth)
- `(admin)/` - Authenticated user dashboard and account management

**Key Configuration Files**:
- `src/config.ts` - Site metadata and feature flags
- `src/DatabaseDefinitions.ts` - Generated Supabase types
- `src/hooks.server.ts` - Auth middleware and session handling

### Database Schema
- `profiles` table - User profile data with unsubscribed field
- `contact_requests` table - Contact form submissions
- Authentication handled by Supabase Auth (users table)

### Authentication Flow
1. OAuth/email auth handled by Supabase Auth UI components
2. Session management via server hooks with JWT validation
3. Route protection using `locals.safeGetSession()`
4. Auth callbacks handled at `/auth/callback`

### Email System
- Admin emails: `sendAdminEmail()` for notifications
- User emails: `sendUserEmail()` with unsubscribe checking
- Templates: Handlebars files in `src/lib/emails/`
- Provider: Resend (requires `PRIVATE_RESEND_API_KEY`)

### Blog Engine
- Static posts defined in `src/routes/(marketing)/blog/posts.ts`
- Post pages in `src/routes/(marketing)/blog/(posts)/`
- RSS feed generation at `/blog/rss.xml`
- SEO optimization with meta tags

### Search Implementation
- Build-time indexing of prerendered HTML pages
- Fuse.js for client-side search functionality
- API endpoint at `/search/api.json`
- Configurable exclusions in `src/lib/build_index.ts`

### Environment Configuration
Required environment variables:
- `PUBLIC_SUPABASE_URL` - Supabase project URL
- `PUBLIC_SUPABASE_ANON_KEY` - Supabase anon key
- `PRIVATE_SUPABASE_SERVICE_ROLE` - Supabase service role key
- Optional: `PRIVATE_RESEND_API_KEY`, `PRIVATE_ADMIN_EMAIL`

### OAuth Configuration
- Providers configured in `src/routes/(marketing)/login/login_config.ts`
- Currently supports GitHub OAuth
- Styling matches DaisyUI theme via CSS variables

### Performance Optimizations
- Prerendering for marketing pages and blog posts
- Inline styles threshold: 150KB for faster FCP
- CDN-optimized static assets
- Client-side navigation after initial SSR load

## Development Guidelines

### Route Patterns
- Marketing pages should be prerendered (`prerender = true` in +page.ts)
- Admin pages require authentication checks in +layout.server.ts
- Use SvelteKit's automatic route-based code splitting

### Database Operations
- Use typed Supabase client from `app.d.ts` locals
- Service role client for admin operations
- Row Level Security (RLS) enabled on tables

### Testing
- Unit tests with Vitest
- Email testing in `src/lib/mailer.test.ts`
- Server action testing in `src/routes/(admin)/account/api/page.server.test.ts`

### Styling Conventions
- TailwindCSS for utility-first styling
- DaisyUI components for consistent UI
- CSS custom properties for theme integration
- Mobile-first responsive design