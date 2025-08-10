# Payment System Inventory

This document provides a comprehensive inventory of all payment-related code, files, database structures, and configuration that needs to be removed from the CMSaasStarter project.

## Stripe Integration Files

### Core Stripe Files

- `src/routes/(admin)/account/subscription_helpers.server.ts` - Main Stripe integration helper functions
- `src/routes/(admin)/account/subscribe/[slug]/+page.server.ts` - Stripe checkout session creation
- `src/routes/(admin)/account/(menu)/billing/manage/+page.server.ts` - Stripe billing portal integration

### Pricing Configuration Files

- `src/routes/(marketing)/pricing/pricing_plans.ts` - Pricing plan configurations with Stripe IDs
- `src/routes/(marketing)/pricing/pricing_module.svelte` - Pricing display component
- `src/routes/(marketing)/pricing/+page.svelte` - Pricing page
- `src/routes/(marketing)/pricing/+page.ts` - Pricing page configuration

## Payment-Related Routes and Pages

### Billing Routes

- `/src/routes/(admin)/account/(menu)/billing/` - Entire billing directory
  - `+page.server.ts` - Billing page server logic
  - `+page.svelte` - Billing page UI
  - `manage/+page.server.ts` - Billing management portal

### Subscription Routes

- `/src/routes/(admin)/account/subscribe/` - Entire subscription directory
  - `[slug]/+page.server.ts` - Dynamic subscription checkout

### Plan Selection Routes

- `/src/routes/(admin)/account/select_plan/` - Plan selection page
  - `+page.svelte` - Plan selection UI

### Pricing Routes

- `/src/routes/(marketing)/pricing/` - Entire pricing directory
  - All files in this directory

## Database Schema Elements

### Payment Tables

- `stripe_customers` table in `supabase/migrations/20240730010101_initial.sql`
  - Fields: `user_id`, `updated_at`, `stripe_customer_id`
  - Row Level Security policies enabled

### Database Migration Files

- `supabase/migrations/20240730010101_initial.sql` - Contains stripe_customers table creation

## Environment Variables

### Stripe Configuration

- `PRIVATE_STRIPE_API_KEY` - Referenced in multiple files:
  - `src/routes/(admin)/account/subscription_helpers.server.ts`
  - `src/routes/(admin)/account/subscribe/[slug]/+page.server.ts`

### Package Dependencies

- `stripe` package in `package.json` (version 13.3.0)
- `stripe` package in `package-lock.json`

## UI Components with Payment References

### Navigation Links

- `src/routes/(marketing)/+layout.svelte` - Contains pricing page links in:
  - Main navigation menu
  - Mobile menu
  - Footer links

### Account Navigation

- `src/routes/(admin)/account/(menu)/+layout.svelte` - Billing menu item

### Payment UI Components

- `src/routes/(admin)/account/(menu)/billing/+page.svelte` - Billing management UI
- `src/routes/(admin)/account/select_plan/+page.svelte` - Plan selection UI
- `src/routes/(marketing)/pricing/pricing_module.svelte` - Pricing display component

### Homepage Features

- `src/routes/(marketing)/+page.svelte` - References to:
  - Subscriptions feature
  - Pricing page feature
  - Billing portal feature
  - Stripe integration mentions

## Configuration and Documentation

### Application Configuration

- `src/config.ts` - Website description mentions Stripe integration

### Documentation

- `README.md` - Multiple references to:
  - Billing/subscriptions features
  - Stripe integration
  - Pricing page
  - Payment setup instructions
  - Stripe configuration steps

## Email Templates

### Subscription References

- `src/lib/emails/welcome_email_html.hbs` - Unsubscribe link references
- `src/lib/emails/welcome_email_text.hbs` - Unsubscribe link references

## Code References and Imports

### Stripe SDK Imports

- `import Stripe from "stripe"` in:
  - `src/routes/(admin)/account/subscription_helpers.server.ts`
  - `src/routes/(admin)/account/subscribe/[slug]/+page.server.ts`

### Pricing Plan Imports

- `import { pricingPlans } from "../../(marketing)/pricing/pricing_plans"` in:
  - `src/routes/(admin)/account/subscription_helpers.server.ts`
  - `src/routes/(admin)/account/(menu)/billing/+page.svelte`
  - `src/routes/(admin)/account/select_plan/+page.svelte`

### Component Imports

- `import PricingModule` in:
  - `src/routes/(admin)/account/select_plan/+page.svelte`
  - `src/routes/(admin)/account/(menu)/billing/+page.svelte`

## Subscription Logic and Access Controls

### Subscription Helper Functions

- `getOrCreateCustomerId()` - Creates Stripe customers
- `fetchSubscription()` - Fetches user subscription status
- Various subscription management functions in `subscription_helpers.server.ts`

### Access Control Logic

- Subscription status checks in authentication middleware
- Feature gating based on subscription tiers
- API rate limiting based on subscription status

## Sponsor Content

### README.md Sponsor References

- No explicit sponsor sections found in current README
- No "Kiln AI" references found in codebase
- Package-lock.json contains standard npm package sponsor links (not project-specific)

## Summary

### Files to Delete Completely

1. `src/routes/(admin)/account/subscription_helpers.server.ts`
2. `src/routes/(admin)/account/subscribe/` (entire directory)
3. `src/routes/(admin)/account/(menu)/billing/` (entire directory)
4. `src/routes/(admin)/account/select_plan/` (entire directory)
5. `src/routes/(marketing)/pricing/` (entire directory)

### Files to Modify

1. `supabase/migrations/20240730010101_initial.sql` - Remove stripe_customers table
2. `package.json` - Remove stripe dependency
3. `src/routes/(marketing)/+layout.svelte` - Remove pricing links
4. `src/routes/(admin)/account/(menu)/+layout.svelte` - Remove billing menu
5. `src/routes/(marketing)/+page.svelte` - Remove payment feature references
6. `src/config.ts` - Remove Stripe from description
7. `README.md` - Remove payment/billing documentation
8. Email templates - Remove subscription references

### Environment Variables to Remove

1. `PRIVATE_STRIPE_API_KEY`

### Database Changes Required

1. Remove `stripe_customers` table creation from migration
2. Remove any foreign key constraints to payment tables
