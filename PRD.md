# Product Requirements Document (PRD)
# OTT Streaming Platform - "MahuaPlay"

**Document Version**: 3.2
**Last Updated**: 19 February 2026
**Author**: MahuaPlay Product Team
**Status**: Ready for Development

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Product Features](#2-product-features)
   - **USER APP FEATURES:**
   - 2.1 [User Management](#21-user-management) - Auth, Profiles, Subscriptions
   - 2.2 [Home Page & Browse](#22-home-page--browse-user-app) - Home, Content Details, Categories, Watchlist, Settings
   - 2.3 [Video Player & Playback](#23-video-player--playback-controls) - Player UI, Controls, Quality, Subtitles
   - 2.4 [Search & Discovery](#24-search--discovery) - Search, Recommendations
   - **ADMIN/BACKEND FEATURES:**
   - 2.5 [Content Management System (CMS)](#25-content-management-system-cms) - Upload, Transcoding, Metadata
   - 2.6 [Social Features](#26-social-features) - Ratings, Reviews, Share
   - 2.7 [Advertisement System](#27-advertisement-management-system) - Ads for free tier
   - 2.8 [Notifications](#28-notifications) - Push, Email, SMS
   - 2.9 [Admin Portal](#29-admin-portal-web-dashboard) - Comprehensive Admin Dashboard
3. [Technical Architecture](#3-technical-architecture)
   - 3.1 [High-Level Architecture](#31-high-level-architecture)
   - 3.2 [Microservices Architecture](#32-microservices-architecture)
   - 3.3 [Apache Kafka Event Streaming](#33-apache-kafka-event-streaming-architecture)
   - 3.4 [CDN & Video Delivery Infrastructure](#34-cdn--video-delivery-infrastructure-india-focused)
   - 3.5 [Database Schema](#35-database-schema-core-tables)
   - 3.6 [Third-Party Service Integration (Hybrid)](#36-third-party-service-integration-hybrid-architecture)
4. [Non-Functional Requirements](#4-non-functional-requirements)
5. [Platform Support](#5-platform-support)
6. [Analytics & Metrics](#6-analytics--metrics)
7. [Monetization](#7-monetization)
8. [Roadmap](#8-roadmap)
9. [Success Criteria](#9-success-criteria)
10. [Risks & Mitigations](#10-risks--mitigations)
11. [Appendix](#11-appendix)
12. [Tech Stack Summary](#12-tech-stack-summary-for-development-team)
13. [Integration Matrix & Developer Reference](#13-integration-matrix--developer-reference)

---

## Target Market

| Parameter | Details |
|-----------|---------|
| **Primary Regions** | Bihar, Odisha, Gujarat, Maharashtra (India) |
| **Languages** | Hindi, Marathi, Gujarati, Odia, Bhojpuri, English |
| **Expected Scale** | 1 - 10 Million concurrent users |
| **Primary Device** | Mobile (Android 80%, iOS 15%, Web 5%) |
| **Network** | 4G LTE dominant, some 5G in urban, 3G in rural |

---

## Revision History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 11 Feb 2026 | Product Team | Initial PRD with core features |
| 2.0 | 17 Feb 2026 | Product Team | Added CDN, CMS, Payment, Search, Playback sections |
| 2.1 | 18 Feb 2026 | Product Team | Added Kafka, scaled to 1M+ users |
| 3.0 | 18 Feb 2026 | Product Team | Added Hybrid Architecture (Bunny.net + AWS), Android integration |
| 3.1 | 19 Feb 2026 | Product Team | Added comprehensive Admin Portal section (2.9) with 18 subsections |
| 3.2 | 19 Feb 2026 | Product Team | Added Integration Matrix & Developer Reference, Kafka event schemas, API quick reference |

---

## 1. Executive Summary

### 1.1 Product Vision
MahuaPlay is a next-generation Over-The-Top (OTT) streaming platform that delivers high-quality video content to users across multiple devices. Similar to Netflix, it provides on-demand movies, TV shows, documentaries, and original content with a personalized viewing experience.

### 1.2 Objectives
- Deliver seamless streaming experience across devices
- Provide personalized content recommendations
- Support multiple subscription tiers
- Enable content creators to upload and monetize content
- Achieve 99.9% uptime and <2 second video start time

### 1.3 Target Audience
| Segment | Description | Age Group |
|---------|-------------|-----------|
| Primary | Entertainment seekers, binge watchers | 18-45 |
| Secondary | Families with children | 25-50 |
| Tertiary | Sports and documentary enthusiasts | 30-60 |

---

## 2. Product Features

### 2.1 User Management


#### 2.1.1 Authentication & Login

**Splash Screen Flow:**

| Step | Action | Next Screen |
|------|--------|-------------|
| 1 | App opens, shows MahuaPlay logo | - |
| 2 | Check if user has valid auth token | - |
| 3a | Token valid | Home Screen |
| 3b | Token invalid/missing | Welcome Screen |

**Welcome Screen:**

| Element | Description |
|---------|-------------|
| Logo | MahuaPlay logo at top center |
| Tagline | "Watch Movies, Shows & More" |
| Button 1 | [Continue with Google] - Primary login option |
| Button 2 | [Continue with Phone] - OTP-based login |
| Button 3 | [Login with Email] - Email/password login |
| Link | "Don't have an account? Sign Up" |
| Footer | Terms of Service & Privacy Policy links |

**Phone OTP Login Flow:**

| Step | Screen | User Action |
|------|--------|-------------|
| 1 | Enter Phone | User enters mobile number (+91 format) |
| 2 | Send OTP | App sends 6-digit OTP via SMS |
| 3 | Enter OTP | User enters the 6 digits |
| 4 | Verify | App verifies OTP and logs in user |

- Resend OTP available after 30 seconds
- OTP expires after 5 minutes
- Max 3 attempts, then temporary block

**Email/Password Login:**

| Field | Description |
|-------|-------------|
| Email | Text input for email address |
| Password | Password input with show/hide toggle |
| Forgot Password | Link to reset password via email |
| Login Button | Submit credentials |

**Authentication Logic:**

| Action | Backend Behavior |
|--------|------------------|
| Google Login | OAuth flow, returns token, create/login user |
| Phone OTP | Send OTP via SMS gateway, verify, issue JWT |
| Email Login | Validate credentials against database, issue JWT |
| Forgot Password | Send reset link to registered email |
| Session | JWT token stored securely, auto-refresh before expiry |
| Logout | Clear token from device, redirect to Welcome screen |

---

#### 2.1.2 User Profiles

**Profile Selection Screen (After Login):**

- Shows "Who's Watching?" heading
- Displays all profiles as circular avatars with names
- Includes "Add Profile" button (if less than 5 profiles)
- "Manage Profiles" link at bottom

**Profile Features:**

| Feature | Description |
|---------|-------------|
| Multiple Profiles | Up to 5 profiles per account |
| Profile Name | Custom display name (max 20 characters) |
| Avatar | Choose from 20+ preset avatar images |
| Language | Preferred content language (Hindi, Marathi, Gujarati, Odia, Bhojpuri, English) |
| Kids Profile | Toggle to enable child-safe mode (only U/UA content visible) |
| Profile PIN | Optional 4-digit PIN to lock profile |

**Add/Edit Profile Screen:**

| Element | Description |
|---------|-------------|
| Avatar | Tap to change from preset options |
| Name Input | Enter profile name |
| Language Dropdown | Select preferred language |
| Kids Toggle | Enable/disable kids mode |
| PIN Toggle | Enable/disable profile PIN |
| Save Button | Save profile changes |
| Delete Button | Delete profile (not available for primary profile) |

**Profile Behavior:**

| Scenario | Behavior |
|----------|----------|
| New Account | Auto-creates first profile with account holder's name |
| Profile Switch | Tap profile icon on Home > Select profile > Loads that profile's Home |
| Kids Profile | Shows only U/UA rated content, hides mature content completely |
| PIN Protected | Requires 4-digit PIN before entering profile |
| Delete Profile | Confirmation required, watchlist and history deleted |

**Per-Profile Data:**

- Watchlist (My List)
- Watch History
- Continue Watching progress
- Recommendations
- Language preference
- Playback settings (quality, subtitles)

---

#### 2.1.3 Subscription Plans (India Pricing)

**Subscription Tiers:**

| Feature | Free | Basic | Standard | Premium |
|---------|------|-------|----------|---------|
| **Price (Monthly)** | Rs.0 | Rs.149 | Rs.299 | Rs.499 |
| **Price (Yearly)** | Rs.0 | Rs.999 (44% off) | Rs.1,999 (44% off) | Rs.3,499 (42% off) |
| **Concurrent Screens** | 1 | 1 | 2 | 4 |
| **Video Quality** | 480p | 720p | 1080p | 4K HDR |
| **Devices** | Mobile only | Mobile + Web | All devices | All devices |
| **Ads** | Yes (4-5/hr) | Yes (2/hr) | No | No |
| **Profiles** | 1 | 2 | 4 | 6 |
| **Content Access** | Limited library | Full library | Full + Early access | Full + Exclusives |
| **Audio Quality** | Stereo | Stereo | 5.1 Surround | Dolby Atmos |
| **Watch Party** | No | No | Yes | Yes |

---

#### 2.1.4 Payment Gateway Integration

| Parameter | Details |
|-----------|---------|
| **Gateway** | Sabpaisa |
| **Integration Type** | Redirect-based (hosted payment page) |
| **Supported Methods** | UPI, Cards, Net Banking, Wallets |

**Payment Flow:**

| Step | Action |
|------|--------|
| 1 | User selects subscription plan |
| 2 | App redirects to Sabpaisa payment page |
| 3 | User completes payment on Sabpaisa |
| 4 | Sabpaisa redirects back to app |
| 5 | Webhook confirms payment to backend |
| 6 | Subscription activated |

> **Note:** Payment UI is handled entirely by Sabpaisa. MahuaPlay only integrates via redirect URL and webhook callback.

---

#### 2.1.5 Subscription Lifecycle Management

**Subscription States:**

| State | Description | User Access | Duration |
|-------|-------------|-------------|----------|
| **Trial** | Free trial period | Full Standard access | 7 days |
| **Active** | Paid subscription | Based on plan | Until renewal |
| **Paused** | User-initiated pause | Frozen | Max 30 days |
| **Grace** | Payment failed, retrying | Continues access | 7 days |
| **Cancelled** | User cancelled | Access until period ends | Until end date |
| **Expired** | Subscription ended | Downgrade to Free | Immediate |

**State Transitions:**

| From State | To State | Trigger |
|------------|----------|---------|
| Trial | Active | User pays for subscription |
| Trial | Expired | Trial ends without payment |
| Active | Paused | User pauses subscription |
| Active | Cancelled | User cancels subscription |
| Active | Grace | Payment fails |
| Paused | Active | User resumes subscription |
| Grace | Active | Payment succeeds |
| Grace | Expired | All retry attempts fail |
| Cancelled | Expired | Subscription period ends |

---

#### 2.1.6 Billing & Renewal System

**Billing Cycles:**

| Cycle | Description | Discount |
|-------|-------------|----------|
| Monthly | Billed on same date each month | None |
| Quarterly | Billed every 3 months | 10% off |
| Yearly | Billed annually | ~44% off |

**Auto-Renewal Flow:**

| Day | Action |
|-----|--------|
| Day -7 | Reminder email/push notification |
| Day -3 | Reminder with payment method check |
| Day -1 | Final reminder |
| Day 0 | Attempt payment |
| Day 0 (Success) | Renew subscription, send confirmation |
| Day 0 (Failure) | Enter Grace period |
| Day 1 | Retry payment |
| Day 3 | Retry + email alert |
| Day 5 | Retry + SMS alert |
| Day 7 | Subscription expired, downgrade to Free |

**Payment Retry Logic:**

- Retry Attempts: 4 (Day 0, 1, 3, 5)
- Retry Time: 10:00 AM IST
- Prompt user to add backup payment method
- UPI Mandate available for guaranteed recurring payments

---

#### 2.1.7 Subscription Management UI (User)

**My Subscription Page Elements:**

| Section | Contents |
|---------|----------|
| Current Plan | Plan name, price, next billing date, payment method |
| Plan Benefits | List of features (screens, quality, profiles, etc.) |
| Payment Methods | Saved cards/UPI with add/remove options |
| Billing History | Date, description, amount, status, invoice download |

**User Subscription Actions:**

| Action | Description | Rules |
|--------|-------------|-------|
| **Upgrade** | Move to higher plan | Immediate, prorated billing |
| **Downgrade** | Move to lower plan | Effective from next billing cycle |
| **Pause** | Temporarily stop | Max 30 days, 2x per year |
| **Resume** | Restart paused plan | Billing resumes immediately |
| **Cancel** | End subscription | Access continues until period end |
| **Reactivate** | Restart after cancel | New billing cycle starts |

---

#### 2.1.8 Promo Codes & Discounts

**Promo Code Types:**

| Type | Example Code | Discount |
|------|--------------|----------|
| Percentage Off | SAVE50 | 50% off first month |
| Flat Discount | FLAT100 | Rs.100 off |
| Free Trial Extend | FREETRIAL | 30 days free (vs 7) |
| Free Months | ANNUAL2FREE | 2 months free on yearly |
| Referral | REF_USER123 | Both get 1 month free |
| Partner | JIO_BUNDLE | 3 months free with Jio plan |
| Seasonal | DIWALI2026 | 40% off yearly plans |
| Student | STUDENT25 | 25% off (verified) |

**Promo Code Rules:**

- One promo code per subscription
- Cannot combine with other offers
- Valid for specific plans only (configurable)
- Usage limits (total uses, per-user)
- Date validity (start/end dates)
- First-time subscribers only (configurable)
- Auto-expire after X days unused

**Referral Program:**

| Step | Action |
|------|--------|
| 1 | User A shares referral link |
| 2 | User B signs up with referral link |
| 3 | User B gets 30 days free trial (vs 7 days) |
| 4 | User B subscribes to paid plan |
| 5 | User A gets 1 month free added to subscription |

- Limit: Max 12 referrals per year (12 free months)

---

#### 2.1.9 Content Entitlement System

**Entitlement Matrix:**

| Content Type | Free | Basic | Standard | Premium |
|--------------|------|-------|----------|---------|
| Free Library | Yes | Yes | Yes | Yes |
| Standard Library | No | Yes | Yes | Yes |
| Premium/Exclusive | No | No | No | Yes |
| Early Access (7 days) | No | No | Yes | Yes |
| Live Sports | No | No | Yes | Yes |
| 4K/HDR Content | No | No | No | Yes |
| Dolby Atmos Audio | No | No | No | Yes |
| Ad-Free | No | No | Yes | Yes |

**Content Tagging:**

Each content item has these access properties:
- access_tier: free, basic, standard, or premium
- is_exclusive: true/false
- early_access_days: number of days early access
- available_qualities: list of quality options
- downloadable: true/false
- ad_supported: true/false
- premium_audio: true/false (Dolby Atmos)

**Access Check Flow:**

| Step | Action |
|------|--------|
| 1 | User requests to play content |
| 2 | Check user subscription status and tier |
| 3a | Has Access: Generate playback token, apply quality limits |
| 3b | No Access: Show upgrade prompt with 30-second trailer preview |

---

#### 2.1.10 Concurrent Stream Management

**Stream Limits by Plan:**

| Plan | Concurrent Streams | Registered Devices | Max Quality |
|------|-------------------|-------------------|-------------|
| Free | 1 | 2 | 480p |
| Basic | 1 | 3 | 720p |
| Standard | 2 | 5 | 1080p |
| Premium | 4 | 10 | 4K |

**Stream Enforcement:**

| Scenario | Behavior |
|----------|----------|
| Under limit | Allow stream to start |
| At limit | Show message: "You're watching on X devices. Stop watching on another device to continue here." |
| User action | Show list of active streams with option to stop other streams |
| Alternative | Offer upgrade plan option |

**Device Management Features:**

- Max registered devices per plan
- Device naming (Living Room TV, Dad's Phone, etc.)
- Remove device (frees up slot)
- Sign out all devices (security feature)
- Device activity log (last used, location)

---

#### 2.1.11 Invoice & Tax Management

**Invoice Contents:**

| Field | Example |
|-------|---------|
| Invoice Number | INV-2026-0012345 |
| Date | 15 February 2026 |
| Company GSTIN | 27XXXXX1234X1ZX |
| Bill To | User name and address |
| Description | Standard Plan (Monthly) |
| Base Amount | Rs.253.39 |
| IGST @ 18% | Rs.45.61 |
| Total | Rs.299.00 |
| Payment Method | Visa ending 4242 |
| Transaction ID | pay_XXXXXXXXXXXXXX |

**Tax Rules (India):**

- GST Rate: 18% on digital services
- IGST for inter-state (user in different state than company)
- CGST + SGST for intra-state
- GST included in displayed price (Rs.299 includes GST)
- B2B invoices: Show GST breakup, accept GSTIN

---

#### 2.1.12 Subscription Database Schema

```sql
-- Subscription Plans (Admin defined)
CREATE TABLE subscription_plans (
    id UUID PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    slug VARCHAR(50) UNIQUE NOT NULL,
    price_monthly DECIMAL(10,2) NOT NULL,
    price_quarterly DECIMAL(10,2),
    price_yearly DECIMAL(10,2),
    currency VARCHAR(3) DEFAULT 'INR',
    max_screens INTEGER DEFAULT 1,
    max_devices INTEGER DEFAULT 3,
    max_profiles INTEGER DEFAULT 2,
    max_quality VARCHAR(10) DEFAULT '720p',
    has_ads BOOLEAN DEFAULT TRUE,
    features JSONB,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT NOW()
);

-- User Subscriptions
CREATE TABLE user_subscriptions (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    plan_id UUID REFERENCES subscription_plans(id),
    billing_cycle VARCHAR(20) NOT NULL,
    status VARCHAR(20) NOT NULL,
    current_period_start TIMESTAMP NOT NULL,
    current_period_end TIMESTAMP NOT NULL,
    cancel_at_period_end BOOLEAN DEFAULT FALSE,
    cancelled_at TIMESTAMP,
    pause_start TIMESTAMP,
    pause_end TIMESTAMP,
    trial_start TIMESTAMP,
    trial_end TIMESTAMP,
    promo_code_id UUID,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Active Streams (for concurrent limit)
CREATE TABLE active_streams (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    profile_id UUID REFERENCES profiles(id),
    device_id VARCHAR(255) NOT NULL,
    device_name VARCHAR(100),
    device_type VARCHAR(50),
    content_id UUID,
    stream_token VARCHAR(255) UNIQUE,
    started_at TIMESTAMP DEFAULT NOW(),
    last_heartbeat TIMESTAMP DEFAULT NOW(),
    ip_address INET,
    UNIQUE(user_id, device_id)
);
```

---

### 2.2 Home Page & Browse (User App)

#### 2.2.1 Home Page Layout

**Home Screen Structure:**

| Section | Position | Description |
|---------|----------|-------------|
| Header | Top | Logo, Search icon, Profile icon |
| Hero Banner | Below header | Featured content (3-5 items, auto-rotate) with Play, My List, Info buttons |
| Continue Watching | 1st row | Incomplete content with progress bars |
| Trending Now | 2nd row | Most watched in last 24-48 hours |
| New Releases | 3rd row | Content added in last 7 days |
| Top 10 in India | 4th row | Most watched overall with ranking numbers |
| Personalized | 5th row | "Because you watched X" recommendations |
| Genre Rows | Dynamic | Action, Comedy, Drama based on user taste |
| Language Rows | Dynamic | Bhojpuri, Marathi, Gujarati, Odia content |
| Bottom Nav | Bottom | Home, Search, Shows, Movies, Settings |

**Row Behavior:**

- Horizontal scroll (swipe left/right)
- Shows 5-6 items visible at a time
- Tap thumbnail: Go to Content Details page
- Long press: Quick actions (Add to list, Share)
- "See All": Opens full grid view of that category

---

#### 2.2.2 Content Details Page

**Content Details Elements:**

| Element | Description |
|---------|-------------|
| Header | Back button, Share button |
| Trailer/Poster | Large image/video with Play button overlay |
| Title | Large, prominent text |
| Metadata | Year, Rating (UA/A/U), Duration, Genres |
| User Rating | Star rating if available |
| Languages | Available audio options |
| Action Buttons | Play, Add to My List |
| Synopsis | 2-3 lines expandable with "Read More" |
| Cast | Horizontal scroll with photos and names |
| Director/Writer | Text credits |
| More Like This | Row of similar content |

**For TV Shows/Series:**

| Additional Element | Description |
|-------------------|-------------|
| Season Selector | Dropdown to choose season |
| Episode List | Vertical list with thumbnail, title, duration, description |
| Episode Play | Individual play button per episode |

**Content Metadata:**

| Field | Display |
|-------|---------|
| Title | Large, prominent |
| Year | Release year |
| Rating | UA, A, U (CBFC) with age indicator |
| Duration | Hours & minutes |
| Genres | Comma-separated |
| User Rating | Star rating |
| Languages | Audio options |

---

#### 2.2.3 Browse & Categories

**Browse Screen Sections:**

| Section | Items |
|---------|-------|
| Categories | Movies, TV Shows, Sports, Kids, News, Documentaries |
| Genres | Action, Comedy, Drama, Romance, Thriller, Horror, Family, Sci-Fi, Crime, Musical |
| Languages | Hindi, Marathi, Gujarati, Odia, Bhojpuri, English, Telugu |

**Category/Genre Listing Page:**

- Header with back button, category name, filter icon
- Sort dropdown (Popular, New, A-Z, Release Year, Rating)
- Grid of content thumbnails (4 columns)
- Infinite scroll or "Load More" button

**Filter Options:**

| Filter | Options |
|--------|---------|
| Sort By | Popular, New, A-Z, Release Year, Rating |
| Language | Hindi, Marathi, Gujarati, Odia, Bhojpuri, English |
| Year | 2024, 2023, 2022, 2020s, 2010s, etc. |
| Rating | All, U, UA, A |

---

#### 2.2.4 Watchlist (My List)

**My List Screen:**

- Header with back button and "My List" title
- Item count display (e.g., "12 items in your list")
- Grid view of saved content thumbnails

**Watchlist Features:**

| Feature | Behavior |
|---------|----------|
| Add to List | Tap "+" on content details or long-press thumbnail |
| Remove | Swipe left on item or tap "-" button |
| Per-Profile | Each profile has separate watchlist |
| Sync | Synced across devices |
| Limit | Unlimited items |

---

#### 2.2.5 Watch History & Continue Watching

**Watch History Logic:**

| Condition | Behavior |
|-----------|----------|
| Position < 95% of duration | Show in "Continue Watching" row with progress bar |
| Position >= 95% | Mark as "Watched", remove from Continue Watching |
| Series episode completed | Auto-add next episode to Continue Watching |

**Continue Watching Row Behavior:**

| Scenario | Behavior |
|----------|----------|
| Movie at 50% | Shows with progress bar, resumes from 50% |
| Series Episode | Shows episode thumbnail, resumes from last position |
| Completed Episode | Auto-shows next episode |
| Completed Series | Removed from continue watching |
| Remove | Tap "X" or long-press and select "Remove from row" |

---

#### 2.2.6 Settings & Preferences

**Settings Screen Layout:**

| Section | Items | Description |
|---------|-------|-------------|
| Account | Profile, Subscription, Payment, Linked Accounts | User account management |
| Playback | Quality, Autoplay, Subtitles | Video playback preferences |
| App Settings | Language, Notifications, Data Saver | App behavior settings |
| Devices | Manage Devices, Sign Out All | Device management |
| Support | Help, Report Problem, Legal | Help and support options |

---

##### 2.2.6.1 Account Settings

**Profile Settings:**

| Element | Description |
|---------|-------------|
| Edit Profile | Opens current profile edit screen |
| Switch Profile | Shows profile selector |
| Manage Profiles | Add/edit/delete profiles |
| Profile PIN | Set or change 4-digit PIN |

**Profile Settings Workflow:**

| Step | Action | Result |
|------|--------|--------|
| 1 | Tap "Profile Settings" | Opens profile management screen |
| 2 | Tap current profile avatar | Opens edit screen for that profile |
| 3 | Edit name/avatar/language | Changes saved on tap "Save" |
| 4 | Toggle "Kids Profile" | Enables/disables child-safe mode |
| 5 | Toggle "Require PIN" | Prompts to set 4-digit PIN |

**Subscription Settings:**

| Element | Description |
|---------|-------------|
| Current Plan | Shows plan name and price |
| Renewal Date | Next billing date |
| Change Plan | Upgrade/downgrade options |
| Billing History | List of past payments |
| Cancel Subscription | Cancel with confirmation |

**Subscription Workflow:**

| Action | Steps | Backend Behavior |
|--------|-------|------------------|
| View Plan | Tap Subscription | Fetch current subscription from API |
| Upgrade | Tap Change Plan > Select higher plan > Confirm | Redirect to Sabpaisa, prorate charges |
| Downgrade | Tap Change Plan > Select lower plan > Confirm | Schedule change for next billing cycle |
| Cancel | Tap Cancel > Confirm reason > Confirm | Set cancel_at_period_end = true |
| View Invoice | Tap Billing History > Tap invoice | Download PDF invoice |

**Payment Methods:**

| Element | Description |
|---------|-------------|
| Saved Cards | List of saved credit/debit cards |
| UPI IDs | Saved UPI payment methods |
| Add Payment | Add new card or UPI |
| Set Default | Mark as primary payment method |
| Remove | Delete saved payment method |

**Payment Methods Workflow:**

| Action | Steps | Validation |
|--------|-------|------------|
| Add Card | Tap Add > Enter card details > Verify OTP | Card number, expiry, CVV validation |
| Add UPI | Tap Add > Enter UPI ID > Verify | UPI ID format validation |
| Set Default | Tap card > Tap "Set as Default" | Only one default allowed |
| Remove | Swipe left > Confirm delete | Cannot remove if only payment method |

**Linked Accounts:**

| Account Type | Description | Actions |
|--------------|-------------|---------|
| Google | Google account for login | Link / Unlink |
| Phone | Mobile number for OTP login | Change number / Verify |
| Email | Email for notifications | Change email / Verify |
| Facebook | Social login (optional) | Link / Unlink |

**Linked Accounts Workflow:**

| Action | Steps | Validation |
|--------|-------|------------|
| Link Google | Tap Link > Google OAuth flow > Confirm | Must not be linked to another account |
| Unlink Google | Tap Unlink > Confirm | Must have alternate login method |
| Change Phone | Enter new number > Verify OTP | Number not already registered |
| Change Email | Enter new email > Verify link | Email not already registered |

---

##### 2.2.6.2 Playback Settings

**Video Quality Settings:**

| Quality | Resolution | Bitrate | Best For |
|---------|------------|---------|----------|
| Auto | Adaptive | Variable | All networks (recommended) |
| Low | 360p | 300 Kbps | Slow 3G, data saving |
| Medium | 480p | 600 Kbps | 3G networks |
| High | 720p | 2.5 Mbps | 4G networks |
| HD | 1080p | 5 Mbps | WiFi, 5G |
| Ultra HD | 4K | 15 Mbps | Premium plan, WiFi only |

**Video Quality Workflow:**

| Step | Action | System Behavior |
|------|--------|-----------------|
| 1 | Tap "Video Quality" | Shows quality options |
| 2 | Select quality level | Saves preference to profile |
| 3 | Start playback | Uses selected quality (or lower if network slow) |
| 4 | Auto mode active | ABR adjusts quality every 10 seconds based on bandwidth |

**Quality Restrictions:**

| Plan | Max Quality | Ultra HD Access |
|------|-------------|-----------------|
| Free | 480p | No |
| Basic | 720p | No |
| Standard | 1080p | No |
| Premium | 4K | Yes |

**Autoplay Settings:**

| Setting | Description | Default |
|---------|-------------|---------|
| Autoplay Next Episode | Automatically play next episode after current ends | ON |
| Autoplay Previews | Play trailer previews while browsing | ON |
| Countdown Timer | Seconds before auto-playing next (5/10/15/20) | 10 seconds |
| Skip Intro | Auto-skip intro when available | OFF |

**Autoplay Workflow:**

| Scenario | Behavior when ON | Behavior when OFF |
|----------|------------------|-------------------|
| Episode ends | 10-second countdown, then plays next | Shows "Next Episode" prompt |
| Browsing Home | Previews play on hover/focus after 2 sec | Static thumbnails only |
| Series finale | Shows recommendations, no autoplay | Shows recommendations |
| Binge mode | Reduces countdown to 5 seconds | No change |

**Subtitle Settings:**

| Setting | Options | Default |
|---------|---------|---------|
| Subtitles | OFF / Hindi / English / Regional | OFF |
| Subtitle Size | Small / Medium / Large | Medium |
| Subtitle Style | White / Yellow / White with background | White with background |
| Subtitle Position | Bottom / Top | Bottom |

**Subtitle Workflow:**

| Step | Action | Result |
|------|--------|--------|
| 1 | Tap "Subtitles" in settings | Opens subtitle preferences |
| 2 | Select default language | Applied to all future playback |
| 3 | Adjust size/style | Preview shown in real-time |
| 4 | Save preferences | Stored per profile |
| 5 | During playback | Can override via player controls |

---

##### 2.2.6.3 App Settings

**App Language:**

| Language | UI Text | Voice Search |
|----------|---------|--------------|
| English | Full support | Yes |
| Hindi | Full support | Yes |
| Marathi | Full support | Yes |
| Gujarati | Full support | Yes |
| Odia | Full support | Yes |
| Bhojpuri | Partial (main UI) | No |

**App Language Workflow:**

| Step | Action | Result |
|------|--------|--------|
| 1 | Tap "App Language" | Shows language list |
| 2 | Select language | Confirmation prompt |
| 3 | Confirm change | App restarts with new language |
| 4 | Backend sync | Language preference saved to profile |

**Content Language:**

| Setting | Description |
|---------|-------------|
| Primary Language | Preferred audio language for content |
| Secondary Languages | Fallback languages if primary not available |
| Show All Content | Include content from all languages |

**Content Language Workflow:**

| Step | Action | Result |
|------|--------|--------|
| 1 | Tap "Content Language" | Shows multi-select language list |
| 2 | Select primary language | Marked with star icon |
| 3 | Select additional languages | Content from these shown on Home |
| 4 | Save | Home feed refreshes with selected languages |

**Notifications Settings:**

| Notification Type | Description | Default |
|-------------------|-------------|---------|
| New Releases | New movies/shows in your languages | ON |
| Continue Watching | Reminders for incomplete content | ON |
| Recommendations | Personalized content suggestions | ON |
| Subscription | Billing, renewal, expiry alerts | ON (cannot disable) |
| Promotions | Offers, discounts, announcements | ON |
| Downloads | Download complete notifications | ON |

**Notifications Workflow:**

| Step | Action | Result |
|------|--------|--------|
| 1 | Tap "Notifications" | Shows notification toggles |
| 2 | Toggle individual types | Immediately saved |
| 3 | "Mute All" option | Disables all except Subscription |
| 4 | Set Quiet Hours | No notifications between set times |

**Notification Delivery:**

| Channel | Types | Timing |
|---------|-------|--------|
| Push (App) | All types | Real-time |
| Email | Subscription, Weekly digest | As scheduled |
| SMS | Subscription alerts only | Critical only |

**Data Saver Mode:**

| Setting | Effect |
|---------|--------|
| OFF | Stream at selected quality |
| ON | Cap quality at 480p, reduce image quality |
| WiFi Only Streaming | Only stream on WiFi, downloads on mobile |

**Data Saver Workflow:**

| Step | Action | Result |
|------|--------|--------|
| 1 | Toggle "Data Saver" ON | Quality capped at 480p |
| 2 | Attempt HD playback | Shows warning, offers to disable |
| 3 | Toggle "WiFi Only" | Playback blocked on mobile data |
| 4 | On mobile data | Prompt to use WiFi or disable setting |

**Estimated Data Usage:**

| Quality | Per Hour | 2-Hour Movie |
|---------|----------|--------------|
| Low (360p) | 0.3 GB | 0.6 GB |
| Medium (480p) | 0.7 GB | 1.4 GB |
| High (720p) | 1.5 GB | 3 GB |
| HD (1080p) | 3 GB | 6 GB |
| 4K | 7 GB | 14 GB |

---

##### 2.2.6.4 Device Management

**Manage Devices:**

| Column | Description |
|--------|-------------|
| Device Name | User-assigned or auto-detected name |
| Device Type | Mobile / Tablet / TV / Web |
| Last Active | Date and time of last use |
| Location | City/Region (approximate) |
| Status | Active / Inactive |

**Device Management Workflow:**

| Action | Steps | Result |
|--------|-------|--------|
| View Devices | Tap "Manage Devices" | Shows all registered devices |
| Rename Device | Tap device > Edit name | Name updated |
| Remove Device | Swipe left > Confirm | Device signed out, slot freed |
| Sign Out All | Tap "Sign Out All Devices" > Confirm | All devices signed out except current |

**Device Limits:**

| Plan | Max Devices | Concurrent Streams |
|------|-------------|-------------------|
| Free | 2 | 1 |
| Basic | 3 | 1 |
| Standard | 5 | 2 |
| Premium | 10 | 4 |

**Device Limit Workflow:**

| Scenario | Behavior |
|----------|----------|
| Adding device when at limit | Prompt to remove existing device |
| Streaming when at concurrent limit | Show active streams, option to stop one |
| Suspicious activity detected | Email alert, prompt to review devices |

---

##### 2.2.6.5 Support & Help

**Help Center:**

| Section | Content |
|---------|---------|
| Getting Started | Account setup, first steps |
| Playback Issues | Buffering, quality, errors |
| Account & Billing | Subscription, payments, invoices |
| Profiles | Managing multiple profiles |
| Technical | Device compatibility, requirements |
| FAQs | Common questions and answers |

**Help Center Workflow:**

| Step | Action | Result |
|------|--------|--------|
| 1 | Tap "Help Center" | Opens in-app help browser |
| 2 | Search or browse topics | Shows relevant articles |
| 3 | Tap article | Full article with images/videos |
| 4 | "Was this helpful?" | Feedback collected |
| 5 | "Contact Support" | Opens support ticket form |

**Report a Problem:**

| Field | Description | Required |
|-------|-------------|----------|
| Category | Playback / Account / Payment / Other | Yes |
| Description | Detailed problem description | Yes |
| Screenshot | Attach screenshot | No |
| Device Info | Auto-captured | Auto |
| Content ID | If playback issue | Auto |

**Report Problem Workflow:**

| Step | Action | Result |
|------|--------|--------|
| 1 | Tap "Report a Problem" | Opens report form |
| 2 | Select category | Shows relevant sub-options |
| 3 | Describe issue | Text input with 500 char limit |
| 4 | Attach screenshot | Optional, max 5 MB |
| 5 | Submit | Ticket created, confirmation shown |
| 6 | Follow-up | Email updates on ticket status |

**Log Out:**

| Option | Description |
|--------|-------------|
| Log Out | Sign out from current device only |
| Log Out All Devices | Sign out from all devices |

**Log Out Workflow:**

| Step | Action | Result |
|------|--------|--------|
| 1 | Tap "Log Out" | Confirmation prompt |
| 2 | Confirm | Clear local token, go to Welcome screen |
| 3 | Profile data | Watchlist, history synced before logout |
| 4 | Downloads | Prompt to keep or delete downloaded content |

**Legal & Info:**

| Item | Description |
|------|-------------|
| Terms of Service | Full terms, opens in browser |
| Privacy Policy | Privacy policy, opens in browser |
| Content Policies | Community guidelines |
| Licenses | Open source licenses used |
| App Version | Current version number |
| Check for Updates | Manual update check |

---

**Settings Data Storage:**

| Setting Type | Storage | Sync |
|--------------|---------|------|
| Profile preferences | Cloud (per profile) | Real-time |
| Playback quality | Cloud (per profile) | Real-time |
| App language | Cloud (per account) | On app restart |
| Notification toggles | Cloud (per account) | Real-time |
| Device settings | Local only | No sync |

---

### 2.3 Video Player & Playback Controls

#### 2.3.1 Video Player Architecture

**Platform-Specific Players**

| Platform | Player Library | DRM Support |
|----------|----------------|-------------|
| Web | Shaka Player | Widevine, PlayReady |
| Android | ExoPlayer | Widevine L1/L3 |
| iOS | AVPlayer | FairPlay |
| Android TV | ExoPlayer + Leanback | Widevine L1 |
| Fire TV | ExoPlayer | Widevine |
| Smart TV | Native + Shaka | PlayReady/Widevine |

**Streaming Protocols**

| Protocol | Use Case | Latency |
|----------|----------|---------|
| HLS | iOS, Safari, default | 20-30 sec |
| DASH | Android, Chrome, Web | 20-30 sec |
| LL-HLS | Live streaming | 2-5 sec |
| CMAF | Unified segments | 20-30 sec |

#### 2.3.2 Player User Interface

**Desktop/Web Player Components**

- **Top Bar (auto-hide)**
  - Back button (returns to browse)
  - Content title
  - Volume, Settings, Fullscreen

- **Center (on hover/tap)**
  - Large Play/Pause button
  - Skip backward/forward (10 sec)

- **Bottom Bar**
  - Progress bar with preview thumbnails
  - Current time / Total duration
  - Playback controls
  - Quality, Subtitles, Audio, Cast, Fullscreen

- **Overlays**
  - Skip Intro button (appears at intro start)
  - Next Episode countdown
  - Buffering spinner

**Mobile Player Gestures**

| Gesture | Action |
|---------|--------|
| Tap | Show/hide controls |
| Double-tap left | Rewind 10 sec |
| Double-tap right | Forward 10 sec |
| Swipe up/down (right) | Volume |
| Swipe up/down (left) | Brightness |
| Swipe left/right | Seek |
| Pinch | Zoom/fit |
| Long press | 2x speed (release to normal) |

**TV Player (Remote Control)**

- **D-Pad Navigation**
  - OK/Select: Play/Pause
  - Left/Right: Seek -10s / +10s
  - Up/Down: Volume or menu navigation
  - Back: Exit player / Show info

- **Dedicated Buttons**
  - Play/Pause: Toggle playback
  - Rewind/Fast Forward: Hold for continuous seek
  - Info: Show content details overlay

#### 2.3.3 Core Playback Controls

**Basic Controls**

| Control | Action | Shortcut (Web) |
|---------|--------|----------------|
| Play/Pause | Toggle playback | Space / K |
| Seek Backward | Jump back 10 sec | Left Arrow / J |
| Seek Forward | Jump ahead 10 sec | Right Arrow / L |
| Seek (Fine) | Jump 5 sec | Shift + Arrow |
| Seek (Large) | Jump 1 minute | Ctrl + Arrow |
| Volume Up | Increase volume 5% | Up Arrow |
| Volume Down | Decrease volume 5% | Down Arrow |
| Mute/Unmute | Toggle mute | M |
| Fullscreen | Toggle fullscreen | F |
| Exit Player | Return to browse | Esc |

**Progress Bar Features**
- Drag to seek
- Click to jump
- Hover preview (thumbnail + timestamp)
- Chapter markers (if available)
- Watched indicator (gray bar)
- Buffered indicator (light bar)
- Skip intro/recap markers

**Preview Thumbnails**
- Generated every 10 seconds during transcoding
- Sprite sheet loaded on player init
- Shows on hover over progress bar

**Volume Control**
- Slider: 0-100%
- Icons: Mute, Low, Medium, High
- Remember per device
- Normalize audio levels (prevent loud ads)

#### 2.3.4 Quality Selection (ABR Controls)

**Video Quality Options**

| Quality | Resolution | Bitrate | Plan Required |
|---------|------------|---------|---------------|
| Auto | Adaptive | Variable | All |
| 4K UHD | 2160p | 15 Mbps | Premium |
| Full HD | 1080p | 5 Mbps | Standard+ |
| HD | 720p | 2.5 Mbps | Basic+ |
| SD | 480p | 1 Mbps | All |
| Low | 360p | 600 Kbps | All |
| Data Saver | 240p | 300 Kbps | All |

**ABR Algorithm (Network-Aware)**
- Monitors bandwidth in real-time
- Buffer-based quality switching
- Prevents excessive quality switches
- Mobile network detection (4G/5G/WiFi)
- India-optimized for variable networks

#### 2.3.5 Subtitles & Closed Captions

**Supported Formats**
- WebVTT (primary)
- SRT (converted on ingest)
- TTML (for accessibility)

**Subtitle Settings**

| Setting | Options |
|---------|---------|
| Font Size | Small, Medium, Large, Extra Large |
| Font Color | White, Yellow, Green, Cyan |
| Background | Black, Semi-transparent, None |
| Edge Style | None, Drop Shadow, Raised, Depressed |
| Font Family | Default, Monospace, Serif |

**Language Selection**
- Auto-detect based on profile language
- Manual selection persists across content
- Off option always available

#### 2.3.6 Audio Track Selection

**Audio Options**

| Track Type | Description |
|------------|-------------|
| Original | Source language audio |
| Hindi | Dubbed audio (most content) |
| English | Dubbed/original audio |
| Descriptive Audio | For visually impaired |
| Dolby 5.1 | Surround sound (Premium) |

**Behavior**
- Default based on profile language preference
- Selection persists for session
- Show audio quality badge (Stereo, 5.1, Atmos)

#### 2.3.7 Skip Intro / Recap / Credits

**Skip Markers**

| Type | Detection | Button Appears |
|------|-----------|----------------|
| Intro | Manual marking in CMS | At intro start |
| Recap | Manual marking in CMS | At recap start |
| Credits | Auto-detect (black + text) | When credits start |

**Next Episode Behavior**
- Auto-play next in 5 seconds during credits
- Cancel button to stay on credits
- Continue Watching updates immediately

#### 2.3.8 Playback Speed Control

**Speed Options**
- 0.5x (slow motion)
- 0.75x
- 1.0x (normal - default)
- 1.25x
- 1.5x
- 1.75x
- 2.0x (fast forward)

**Behavior**
- Audio pitch correction enabled
- Speed resets on new content
- Not available for live content

#### 2.3.9 Picture-in-Picture (PiP)

**Platform Support**

| Platform | PiP Type |
|----------|----------|
| Web | Browser native PiP |
| Android | Android 8+ PiP mode |
| iOS | iOS 14+ native PiP |

**Features**
- Floating mini-player
- Basic controls (play/pause, close)
- Auto-enable when switching apps (mobile)
- Continues playback in background

#### 2.3.10 Casting (Chromecast / AirPlay)

**Supported Protocols**

| Protocol | Platform | Support Level |
|----------|----------|---------------|
| Chromecast | Android, Web | Full |
| AirPlay | iOS, macOS | Full |
| DLNA | Smart TVs | Basic |

**Cast Experience**
- Cast button in player controls
- Device discovery automatic
- Queue management
- Remote control from phone

#### 2.3.11 Continue Watching & Resume

**Resume Points**
- Save position every 10 seconds
- Resume from exact position on return
- Clear position when content completes (>95%)

**Sync Across Devices**
- Real-time sync via API
- Conflict resolution: latest timestamp wins
- Works across all platforms

#### 2.3.12 Binge Mode & Auto-Play

**Auto-Play Next Episode**
- Countdown: 5 seconds
- Cancel button available
- Skip credits automatically
- Respects parental controls

**Binge Mode Settings**
- Auto-play: On/Off (default: On)
- Skip intros automatically: On/Off
- Show "Are you still watching?" after 3 episodes

#### 2.3.13 Sleep Timer

**Timer Options**
- 15 minutes
- 30 minutes
- 45 minutes
- 1 hour
- End of episode
- End of movie

**Behavior**
- Pause playback when timer ends
- Show notification before stopping
- Fade out audio gradually

#### 2.3.14 Parental Controls During Playback

**Content Ratings**

| Rating | Description |
|--------|-------------|
| U | Universal - All ages |
| U/A 7+ | Parental guidance for under 7 |
| U/A 13+ | Parental guidance for under 13 |
| U/A 16+ | Parental guidance for under 16 |
| A | Adults only (18+) |

**Restrictions**
- PIN required for restricted content
- Skip mature scenes option
- Block specific content types

#### 2.3.15 Ad Playback (Free/Basic Tier)

**Ad Types**

| Type | When | Duration | Skippable |
|------|------|----------|-----------|
| Pre-roll | Before content | 15-30 sec | After 5 sec |
| Mid-roll | During content | 15-30 sec | After 5 sec |
| Post-roll | After content | 15-30 sec | Yes |

**Ad Experience**
- Ad counter: "Ad 1 of 2"
- Skip button after mandatory watch time
- No ads on Premium tier
- Reduced ads on Standard tier

#### 2.3.16 Keyboard Shortcuts (Web)

| Key | Action |
|-----|--------|
| Space / K | Play/Pause |
| F | Fullscreen |
| M | Mute |
| J | Rewind 10 sec |
| L | Forward 10 sec |
| Up/Down | Volume |
| Left/Right | Seek |
| 0-9 | Jump to 0%-90% |
| C | Toggle captions |
| Esc | Exit fullscreen |

#### 2.3.17 Error Handling & Buffering

**Error States**

| Error | User Message | Recovery |
|-------|--------------|----------|
| Network Lost | "Check your connection" | Auto-retry on reconnect |
| Stream Unavailable | "Content temporarily unavailable" | Refresh button |
| DRM Error | "Playback error" | Clear cache prompt |
| Geo-blocked | "Not available in your region" | VPN warning |

**Buffering Indicators**
- Spinner appears after 2 seconds of buffering
- Quality auto-downgrades on persistent buffering
- "Experiencing slow connection" message after 10 seconds

#### 2.3.18 Playback Settings (Per Profile)

**Saved Preferences**
- Default video quality
- Default audio language
- Default subtitle language
- Autoplay next episode: On/Off
- Data saver mode: On/Off

---

### 2.4 Search & Discovery

#### 2.4.1 Search Architecture

**Search Stack**
- AWS OpenSearch (Elasticsearch managed)
- Typesense for autocomplete
- Redis for search suggestions cache

**Search Pipeline**
1. Query received from client
2. Query normalization (lowercase, remove special chars)
3. Spell correction and suggestions
4. Search execution across indices
5. Result ranking and personalization
6. Response formatting and caching

#### 2.4.2 Search Features

**Search Types**

| Type | Description |
|------|-------------|
| Title Search | Match content titles |
| Actor/Director | Search by cast/crew |
| Genre | Filter by category |
| Voice Search | Speech-to-text search |
| Barcode/QR | Scan physical media |

**Search UI Components**
- Search bar (top of app)
- Recent searches (last 10)
- Trending searches
- Search suggestions (autocomplete)
- Voice search button (mobile)

**Search Results**
- Grouped by type (Movies, Series, Cast)
- Poster thumbnails
- Match rating
- Quick actions (Play, Add to Watchlist)

#### 2.4.3 Multilingual Search (India-Focused)

**Supported Languages**

| Language | Script | Transliteration |
|----------|--------|-----------------|
| Hindi | Devanagari | Hinglish support |
| Marathi | Devanagari | Yes |
| Gujarati | Gujarati | Yes |
| Odia | Odia | Yes |
| Bhojpuri | Devanagari | Yes |
| English | Latin | Default |

**Features**
- Search in native script or transliterated
- Auto-detect input language
- Cross-language results (search Hindi, get English titles)
- Regional content prioritization

#### 2.4.4 Search Ranking Algorithm

**Ranking Factors**

| Factor | Weight | Description |
|--------|--------|-------------|
| Title Match | 30% | Exact or partial title match |
| Popularity | 25% | Views, engagement metrics |
| Recency | 15% | Recently added content |
| User Relevance | 20% | Based on watch history |
| Quality | 10% | Content rating, reviews |

#### 2.4.5 Voice Search

**Implementation**
- Android: Google Speech Recognition API
- iOS: Speech framework
- Web: Web Speech API

**Features**
- Natural language queries
- "Play [movie name]" support
- "Show me action movies" support
- Multi-language voice recognition

#### 2.4.6 Recommendation Engine

**Recommendation Types**

| Type | Algorithm | Use Case |
|------|-----------|----------|
| Collaborative Filtering | User-user similarity | "Users like you watched" |
| Content-Based | Metadata matching | "Because you watched X" |
| Trending | Popularity decay | "Trending Now" |
| Editorial | Manual curation | "Staff Picks" |

**Input Signals**
- Watch history
- Completion rate
- Ratings given
- Watchlist additions
- Search queries
- Time of day
- Device type

#### 2.4.7 Home Feed Personalization

**Home Page Rows**

| Row | Algorithm | Refresh Rate |
|-----|-----------|--------------|
| Continue Watching | User history | Real-time |
| Trending Now | Popularity | Hourly |
| Because You Watched [X] | Content-based | Daily |
| Top 10 in India | Regional trending | Hourly |
| New Releases | Recency | Daily |
| Your Watchlist | User data | Real-time |

#### 2.4.8 Regional & Language-Based Recommendations

**Regional Prioritization**
- Detect user location (state-level)
- Prioritize regional content
- Hindi content as fallback
- English content available everywhere

**Language Affinity**
- Track preferred audio languages
- Recommend content in those languages
- Show dubbed versions when available

#### 2.4.9 Cold Start Problem Handling

**New User Experience**
1. Show onboarding survey (favorite genres, languages)
2. Display popular content in selected languages
3. Feature editorial picks prominently
4. Learn preferences from first few watches

**New Content Handling**
- Boost new content in relevant categories
- A/B test with different user segments
- Track early engagement signals

#### 2.4.10 Trending & Top 10 Algorithm

**Trending Calculation**
- Views in last 24 hours (weighted: 40%)
- Views in last 7 days (weighted: 30%)
- Completion rate (weighted: 20%)
- Social shares (weighted: 10%)

**Top 10 Lists**
- National Top 10
- Regional Top 10 (per state)
- Genre Top 10
- New Releases Top 10

#### 2.4.11 Search & Recommendation Analytics

**Tracked Metrics**

| Metric | Description |
|--------|-------------|
| Search Success Rate | % searches resulting in play |
| Zero Results Rate | % searches with no results |
| Click-through Rate | % recommendations clicked |
| Conversion Rate | % recommendations watched |
| Discovery Rate | % new content discovered |

#### 2.4.12 Search & Recommendation Database Schema

**Key Tables**

| Table | Purpose |
|-------|---------|
| search_queries | Query logs for analytics |
| search_index | OpenSearch index metadata |
| user_preferences | Learned user preferences |
| recommendations_log | Recommendation tracking |
| content_vectors | ML embedding vectors |

---

### 2.5 Content Management System (CMS) - Admin

#### 2.5.1 Content Types

**Content Hierarchy**
- Movies
  - Bollywood
  - Hollywood
  - Regional (Marathi, Gujarati, Odia, Bhojpuri)
  - International (Dubbed)
- TV Shows / Web Series
  - Original Series
  - Licensed Content
  - Daily Soaps
- Short Films
- Documentaries
- Kids & Family
- Sports (Live & Recorded)
- Music Videos
- User Generated Content (Future)

#### 2.5.2 CMS Admin Dashboard

**Dashboard Modules**

| Module | Description |
|--------|-------------|
| Content Library | Browse, search, filter all content |
| Upload Center | Upload and transcode videos |
| Metadata Manager | Edit titles, descriptions, cast |
| Analytics Reports | Views, engagement, revenue |

**Quick Stats**
- Total Content count
- Movies count
- Series count
- Pending Review count
- Failed Uploads count

**Content Management Features**
- Add/Edit/Delete content
- Bulk import via CSV/Excel
- Schedule publish/unpublish
- Content versioning
- Multi-language metadata
- Category & tag management
- Content grouping (collections, playlists)
- License expiry tracking
- Geo-restriction settings
- Content quality badges (4K, HDR, Dolby)

**User Roles & Permissions**

| Role | Permissions |
|------|-------------|
| Super Admin | Full access, user management |
| Content Admin | Upload, edit, publish content |
| Editor | Edit metadata, cannot publish |
| Reviewer | Review & approve content |
| Analyst | View-only, reports access |

#### 2.5.3 Video Upload System

**Supported Formats**
- Video: MP4, MOV, MKV, AVI, WMV, ProRes
- Codec: H.264, H.265/HEVC, ProRes 422/4444
- Max File Size: 100 GB
- Max Resolution: 4K (3840x2160)
- Max Duration: 4 hours

**Upload Methods**
- Web Dashboard (drag & drop)
- Desktop App (bulk upload)
- S3 Direct Upload (for partners)
- API Upload (automated ingestion)

**Chunked Upload (Resumable)**
- Chunk Size: 5 MB
- Parallel Uploads: 3 chunks
- Auto-retry on failure (3 attempts)
- Resume from last successful chunk
- MD5 checksum per chunk
- Progress tracking in real-time

**Upload Validation**

| Stage | Checks |
|-------|--------|
| Pre-Upload | File format, file size, duplicate detection, storage quota |
| Post-Upload | Integrity verification, corruption detection, audio/video stream validation, duration extraction, metadata extraction |

**Upload API Flow**
1. Client initiates upload, receives Upload ID + presigned URLs
2. Client uploads chunks directly to S3
3. Each chunk returns ETag for verification
4. Client signals upload complete
5. Backend verifies and merges chunks
6. Transcoding triggered automatically

#### 2.5.4 Transcoding Pipeline

**Transcoding Engine**: Bunny.net (primary) / AWS MediaConvert (backup)

**Input Analysis**
- Resolution detection
- Frame rate analysis
- HDR/SDR detection
- Audio channels/codec detection

**Output Encoding Profiles (India-Optimized)**

| Profile | Resolution | Bitrate | Codec | Audio | Use Case |
|---------|------------|---------|-------|-------|----------|
| Ultra HD | 2160p (4K) | 15 Mbps | HEVC | AAC | Premium |
| Full HD | 1080p | 5 Mbps | H.264 | AAC | WiFi |
| HD | 720p | 2.5 Mbps | H.264 | AAC | 4G LTE |
| SD | 480p | 1 Mbps | H.264 | AAC | 4G |
| Low | 360p | 600 Kbps | H.264 | AAC | 3G |
| Mobile Saver | 240p | 300 Kbps | H.264 | HE-AAC | Slow networks |

**Audio Tracks**
- Hindi - 128 Kbps AAC Stereo
- Original - 128 Kbps AAC Stereo
- Dolby 5.1 - 384 Kbps (Premium content only)

**Streaming Format Output**
- HLS (for iOS, Safari)
- DASH (for Android, Chrome)
- CMAF (unified)

**Segment Duration**: 6 seconds (optimized for India networks)

#### 2.5.5 Metadata Management

**Required Metadata**

| Field | Type | Required |
|-------|------|----------|
| Title | Text | Yes |
| Original Title | Text | No |
| Description | Long Text | Yes |
| Release Year | Number | Yes |
| Duration | Number | Auto-detected |
| Genre | Multi-select | Yes |
| Content Rating | Select | Yes |
| Language | Multi-select | Yes |

**Cast & Crew**

| Role | Fields |
|------|--------|
| Director | Name, Photo, Bio |
| Cast | Name, Role, Photo |
| Producer | Name |
| Writer | Name |
| Music Director | Name |

**Assets Required**

| Asset | Dimensions | Format |
|-------|------------|--------|
| Poster (Portrait) | 600x900 | JPG/PNG |
| Backdrop (Landscape) | 1920x1080 | JPG/PNG |
| Logo | 400x150 | PNG (transparent) |
| Trailer | Video | MP4 |

#### 2.5.6 Series & Episode Management

**Series Structure**
- Series (parent)
  - Season 1
    - Episode 1
    - Episode 2
  - Season 2
    - Episode 1

**Episode Metadata**
- Episode Number
- Episode Title
- Episode Description
- Thumbnail
- Intro Start/End timestamps
- Recap Start/End timestamps
- Credits Start timestamp

#### 2.5.7 Asset Management

**Image Processing**
- Auto-resize to required dimensions
- WebP conversion for web
- Lazy loading optimized sizes
- CDN caching

**Video Assets**
- Trailer management
- Clip creation
- Preview generation (auto 30-sec clip)

#### 2.5.8 Content Approval Workflow

**Workflow States**

| State | Description | Next States |
|-------|-------------|-------------|
| Draft | Initial upload | Submit for Review |
| In Review | Pending approval | Approved, Rejected |
| Approved | Ready to publish | Published |
| Published | Live on platform | Unpublished |
| Rejected | Needs changes | Draft |
| Unpublished | Removed from view | Published |

**Review Checklist**
- Video quality verified
- Audio sync checked
- Subtitles reviewed
- Metadata complete
- Age rating appropriate
- Rights/license valid

#### 2.5.9 Content Caching Strategy

**Cache Layers**

| Layer | Technology | TTL | Content Type |
|-------|------------|-----|--------------|
| Edge | Bunny CDN | 24 hours | Video segments |
| Regional | CloudFront | 1 hour | Metadata, images |
| Application | Redis | 15 minutes | API responses |
| Database | Query cache | 5 minutes | DB queries |

**Cache Invalidation Triggers**
- Content metadata update
- Content publish/unpublish
- Image replacement
- Bulk content update

#### 2.5.10 Bulk Import & Export

**Import Formats**
- CSV template
- Excel (XLSX)
- JSON API

**Export Options**
- Content catalog (CSV)
- Analytics reports (PDF, Excel)
- Metadata backup (JSON)

#### 2.5.11 Content Lifecycle Management

**Lifecycle States**

| State | Description | Action |
|-------|-------------|--------|
| Active | Currently available | None |
| Expiring Soon | License ending in 30 days | Alert sent |
| Expired | License ended | Auto-unpublish |
| Archived | Removed but preserved | Manual restore |
| Deleted | Permanently removed | Cannot restore |

**Automated Actions**
- License expiry notifications (30, 7, 1 day)
- Auto-unpublish on expiry
- Trending content boost
- Seasonal content surfacing

---

### 2.6 Social Features

#### 2.6.1 User Interactions

| Feature | Description |
|---------|-------------|
| Ratings | 1-5 star rating per content |
| Reviews | Text reviews with moderation |
| Likes | Quick reaction to content |
| Share | Share to WhatsApp, Facebook, Twitter, Copy Link |

#### 2.6.2 Watch Party

| Feature | Description |
|---------|-------------|
| Host | Create watch party room |
| Invite | Share link with friends |
| Sync | Synchronized playback |
| Chat | Real-time text chat |
| Reactions | Emoji reactions during playback |

---

### 2.7 Advertisement Management System

#### 2.7.1 Ad-Supported Tiers

| Plan | Ad Experience |
|------|---------------|
| Free | Full ads (pre-roll, mid-roll, post-roll) |
| Basic | Reduced ads (pre-roll only) |
| Standard | No ads |
| Premium | No ads |

#### 2.7.2 Ad Types

| Type | Format | Duration | Placement |
|------|--------|----------|-----------|
| Pre-roll | Video | 15-30 sec | Before content |
| Mid-roll | Video | 15-30 sec | During content (every 20 min) |
| Post-roll | Video | 15-30 sec | After content |
| Banner | Image | - | Browse pages |
| Sponsored | Content | - | Home feed |

#### 2.7.3 Ad Insertion Methods

| Method | Description | Use Case |
|--------|-------------|----------|
| SSAI | Server-side ad insertion | Primary (bypasses ad blockers) |
| CSAI | Client-side ad insertion | Fallback |

#### 2.7.4 Ad Targeting Parameters

| Parameter | Description |
|-----------|-------------|
| Demographics | Age, Gender, Location |
| Content | Genre, Rating, Category |
| Behavior | Watch history, Time of day |
| Device | Mobile, TV, Web |

#### 2.7.5 Ad Analytics

| Metric | Description |
|--------|-------------|
| Impressions | Total ad views |
| Completion Rate | % watched to end |
| Click-through Rate | % clicked on ad |
| Revenue | Earnings from ads |

---

### 2.8 Notifications

| Type | Channel | Trigger |
|------|---------|---------|
| New Release | Push, Email | Followed show new season |
| Recommendations | Push | Weekly personalized picks |
| Continue Watching | Push | Remind to finish content |
| Subscription | Email | Payment, renewal, expiry |
| Watch Party | Push | Invitation from friends |

---

### 2.9 Admin Portal (Web Dashboard)

The Admin Portal is a comprehensive web-based dashboard for managing all aspects of the MahuaPlay platform. Every user-facing feature has corresponding admin controls for configuration, monitoring, and management.

---

#### 2.9.1 Admin Portal Overview

**Portal URL**: `admin.mahuaplay.com`

**Technology Stack**
| Component | Technology |
|-----------|------------|
| Frontend | React.js + Next.js |
| UI Framework | Tailwind CSS + Shadcn UI |
| State Management | React Query + Zustand |
| Authentication | AWS Cognito (Admin Pool) |
| API | REST + GraphQL |

**Supported Browsers**
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

**Access Requirements**
- Admin credentials (email + password)
- Two-Factor Authentication (2FA) mandatory
- IP whitelist (optional, for enterprise)
- VPN access (for sensitive operations)

---

#### 2.9.2 Admin Dashboard (Home)

**Dashboard Layout**

| Section | Content |
|---------|---------|
| Header | Logo, Search, Notifications, Profile |
| Sidebar | Navigation menu (collapsible) |
| Main Area | Dashboard widgets, Data tables |
| Footer | Version info, Support link |

**Quick Stats Widgets**

| Widget | Metrics Shown |
|--------|---------------|
| Users | Total users, New today, Active now |
| Content | Total titles, Published, Pending review |
| Revenue | Today's revenue, MTD, Trend chart |
| Subscriptions | Active subs, New today, Churn rate |
| Streaming | Live streams, Peak concurrent, Bandwidth |
| Alerts | System alerts, Failed payments, Reports |

**Recent Activity Feed**
- New user registrations
- Content published
- Subscription changes
- System alerts
- Admin actions

**Quick Actions**
- Add New Content
- Create Notification
- View Reports
- System Health

---

#### 2.9.3 User Management (Admin)

**2.9.3.1 User List & Search**

**Search & Filter Options**

| Filter | Options |
|--------|---------|
| Search | Email, Phone, Name, User ID |
| Status | Active, Suspended, Blocked, Deleted |
| Subscription | Free, Basic, Standard, Premium, Expired |
| Registration Date | Date range picker |
| Last Active | Today, This week, This month, Inactive |
| Region | State/City filter |
| Device | Android, iOS, Web, TV |
| Referral Source | Organic, Referral, Campaign, Social, Partner |

**User List Columns**
- User ID
- Name
- Email / Phone
- Subscription Plan
- Status
- Registration Date
- Last Active
- Actions

**Bulk Actions**
- Export to CSV
- Send notification
- Suspend selected
- Delete selected

**2.9.3.2 User Detail View**

**User Profile Tab**

| Field | Editable | Notes |
|-------|----------|-------|
| Name | Yes | |
| Email | Yes | Requires verification |
| Phone | Yes | Requires OTP |
| Profile Picture | View only | |
| Date of Birth | Yes | |
| Gender | Yes | |
| Preferred Language | Yes | |
| Region | Yes | |
| Referral Source | View only | Organic, Referral, Campaign, Social, Partner |
| Referral Code Used | View only | If referred by another user |
| Referred By | View only | Link to referrer's profile |

**Profiles Tab**
- List all profiles under account
- View profile details (name, avatar, is_kids)
- Delete profile
- View profile watch history

**Subscription Tab**

| Information | Actions |
|-------------|---------|
| Current Plan | Upgrade/Downgrade |
| Status | Active/Expired/Cancelled |
| Start Date | - |
| End Date | Extend subscription |
| Auto-Renewal | Enable/Disable |
| Payment Method | View (masked) |
| Transaction History | View all, Export |

**Subscription Actions**
- Extend subscription (free days)
- Change plan (upgrade/downgrade)
- Cancel subscription
- Issue refund
- Apply promo code manually

**Watch History Tab**
- Content watched with timestamps
- Watch duration per content
- Completion percentage
- Device used
- Export history

**Devices Tab**
- List of registered devices
- Device type, name, last used
- Remove device
- Remove all devices (force logout)

**Activity Log Tab**
- Login history (IP, device, location)
- Subscription changes
- Profile changes
- Support tickets raised

**2.9.3.3 User Actions**

| Action | Description | Requires |
|--------|-------------|----------|
| Edit Profile | Modify user details | Editor role |
| Reset Password | Send password reset email | Editor role |
| Suspend User | Temporary suspension | Moderator role |
| Block User | Permanent block | Admin role |
| Delete User | GDPR deletion | Super Admin |
| Impersonate | View app as user (read-only) | Super Admin |
| Send Notification | Direct push/email to user | Editor role |
| Add Note | Internal notes about user | All roles |

**2.9.3.4 User Segments**

**Create User Segments**
- Define rules based on user attributes
- Save segments for targeting
- Use in notifications, promotions

**Segment Criteria**

| Criteria | Operators |
|----------|-----------|
| Subscription Plan | equals, not equals |
| Registration Date | before, after, between |
| Last Active | within days, not within |
| Watch Time | greater than, less than |
| Content Watched | genre, language, count |
| Region | equals, in list |
| Device | equals, in list |
| Referral Source | equals, in list |

---

#### 2.9.4 Content Management

> Detailed in Section 2.5 CMS. Key admin functions:

**Content Operations**

| Operation | Description |
|-----------|-------------|
| Add Content | Upload video, add metadata |
| Edit Content | Modify metadata, replace video |
| Delete Content | Remove from platform |
| Publish/Unpublish | Control visibility |
| Schedule | Set publish/unpublish dates |
| Feature | Add to featured sections |

**Content List View**

| Column | Sortable | Filterable |
|--------|----------|------------|
| Title | Yes | Search |
| Type | Yes | Movie/Series/Short |
| Genre | No | Multi-select |
| Language | No | Multi-select |
| Status | Yes | Draft/Review/Published |
| Views | Yes | Range |
| Rating | Yes | Range |
| Added Date | Yes | Date range |
| Publish Date | Yes | Date range |

**Content Approval Queue**
- List of pending content
- Review metadata
- Preview video
- Approve / Reject with comments
- Assign to reviewer

---

#### 2.9.5 Subscription & Plan Management

**2.9.5.1 Subscription Plans**

**Plan Configuration**

| Field | Description | Example |
|-------|-------------|---------|
| Plan ID | Unique identifier | plan_premium_monthly |
| Plan Name | Display name | Premium |
| Description | Plan features | 4K, 4 devices, No ads |
| Price | Amount in INR | 499 |
| Billing Cycle | monthly / yearly | monthly |
| Trial Days | Free trial period | 7 |
| Status | Active / Inactive | Active |

**Plan Features Matrix**

| Feature | Free | Basic | Standard | Premium |
|---------|------|-------|----------|---------|
| Max Quality | 480p | 720p | 1080p | 4K |
| Concurrent Streams | 1 | 1 | 2 | 4 |
| Profiles | 1 | 2 | 4 | 5 |
| Ads | Full | Reduced | None | None |
| Downloads | No | No | Yes | Yes |
| Download Limit | - | - | 10 | 25 |

**Plan Actions**
- Create new plan
- Edit existing plan
- Deactivate plan (no new subs)
- Clone plan
- A/B test pricing

**2.9.5.2 Promo Codes**

**Promo Code List View**

| Column | Description |
|--------|-------------|
| Code | Promo code string |
| Type | Discount / Free Trial / Upgrade |
| Value | Discount amount or days |
| Usage | Used / Total limit |
| Status | Active / Expired / Disabled |
| Valid Until | Expiry date |
| Actions | Edit, Disable, Clone, Delete |

**Promo Code Filters**
- Status: Active, Expired, Disabled, All
- Type: Discount, Free Trial, Upgrade
- Date Range: Created date, Expiry date
- Search: Code name

---

**Create Promo Code**

**Step 1: Basic Information**

| Field | Required | Description |
|-------|----------|-------------|
| Promo Code | Yes | Unique code (auto-generate or custom) |
| Display Name | Yes | Internal name for reference |
| Description | No | Internal notes |
| Promo Type | Yes | Discount / Free Trial / Upgrade |

**Step 2: Promo Type Configuration**

*If Type = Discount:*

| Field | Options |
|-------|---------|
| Discount Type | Percentage / Fixed Amount |
| Discount Value | e.g., 50% or ₹100 |
| Max Discount Cap | e.g., Max ₹200 off (for percentage) |
| Apply To | First payment only / All payments / Specific cycle |

*If Type = Free Trial:*

| Field | Options |
|-------|---------|
| Trial Duration | Days (e.g., 7, 14, 30) |
| Trial Plan | Which plan to trial (Basic/Standard/Premium) |
| Auto-convert | Auto-subscribe after trial? Yes/No |
| Payment Required | Collect payment method upfront? Yes/No |

*If Type = Upgrade:*

| Field | Options |
|-------|---------|
| Upgrade To | Target plan (e.g., Premium) |
| Upgrade Duration | Days of free upgrade |
| After Expiry | Revert to original plan / Stay upgraded |

**Step 3: Eligibility Rules**

| Field | Options |
|-------|---------|
| Applicable Plans | All / Select specific plans |
| User Type | All / New users only / Existing users only |
| User Segment | All / Select saved segment |
| Min Purchase | Minimum amount (e.g., ₹299+) |
| Referral Source | All / Specific source (Campaign, Partner) |
| Region | All / Specific states |
| Device | All / Android / iOS / Web |

**Step 4: Usage Limits**

| Field | Description |
|-------|-------------|
| Total Usage Limit | Max total redemptions (e.g., 1000) |
| Per User Limit | Max uses per user (e.g., 1) |
| Per Day Limit | Max redemptions per day |
| Budget Cap | Stop when total discount reaches ₹X |

**Step 5: Validity Period**

| Field | Description |
|-------|-------------|
| Valid From | Start date & time |
| Valid Until | End date & time |
| Active Days | Specific days (e.g., Weekends only) |
| Active Hours | Specific hours (e.g., 6PM-12AM) |

**Step 6: Display Settings**

| Field | Description |
|-------|-------------|
| Show in App | Display on payment screen? Yes/No |
| Banner Text | Promotional text to show |
| Terms & Conditions | T&C text for promo |
| Success Message | Message after redemption |

---

**Promo Code Actions**

| Action | Description |
|--------|-------------|
| Create | Create new promo code |
| Edit | Modify existing promo |
| Clone | Duplicate with new code |
| Disable | Stop accepting redemptions |
| Enable | Reactivate disabled promo |
| Delete | Permanently remove |
| Export | Download promo list |

---

**Promo Code Analytics**

| Metric | Description |
|--------|-------------|
| Total Redemptions | Number of times used |
| Unique Users | Distinct users who redeemed |
| Revenue Impact | Total discount given |
| Conversion Rate | % of users who subscribed |
| Avg Order Value | Average transaction with promo |
| Top Redeemers | Users who used most |
| Daily Trend | Redemption over time chart |

**2.9.5.3 Revenue & Billing**

**Revenue Dashboard**

| Metric | Period Options |
|--------|----------------|
| Total Revenue | Today, Week, Month, Year |
| Revenue by Plan | Breakdown by plan type |
| New vs Renewal | First-time vs recurring |
| Refunds | Total refunded amount |
| Failed Payments | Count and amount |

**Transaction List**

| Column | Description |
|--------|-------------|
| Transaction ID | Unique ID |
| User | Email/Phone |
| Plan | Subscription plan |
| Amount | Paid amount |
| Payment Method | UPI/Card/Wallet |
| Status | Success/Failed/Pending/Refunded |
| Date | Transaction timestamp |

**Transaction Actions**
- View details
- Issue refund (full/partial)
- Retry failed payment
- Export transactions

**2.9.5.4 Refund Management**

**Refund Policy Configuration**
- Auto-refund within X days
- Refund percentage by days used
- Refund approval workflow

**Refund Queue**
- Pending refund requests
- Approve / Reject with reason
- View refund history

---

#### 2.9.6 Home Page & Content Curation

**2.9.6.1 Home Page Layout Manager**

**Row Management**

| Row Type | Configuration | Data Source |
|----------|---------------|-------------|
| Continue Watching | Auto-populated, position only | User watch history |
| Hero Banner | Manual selection, scheduling | Admin curated |
| Featured Carousel | Manual curation | Admin curated |
| Trending | Algorithm + manual override | Views, engagement |
| Top 10 | Algorithm-based | Regional popularity |
| New Releases | Auto, date-based | Release date |
| Genre Row | Auto by genre, reorder | Content metadata |
| Editorial | Fully manual | Admin curated |
| Because You Watched | Personalized per user | Recommendation engine |
| Popular in Your Area | Location-based | Regional views |
| Leaving Soon | Auto, license expiry | Content metadata |
| Just Added | Auto, recently added | Upload date |
| Language Row | Auto by language | Content language |
| Seasonal/Event | Manual, scheduled | Admin curated |
| Watchlist | User's saved content | User data |
| Kids & Family | Auto, kids content | Age rating |
| Award Winners | Manual curation | Admin curated |
| Critics' Choice | Manual curation | Admin curated |

---

**Row Type Details**

**Personalized Rows (Per User)**

| Row | Logic |
|-----|-------|
| Continue Watching | Incomplete content, sorted by last watched |
| Because You Watched | Similar to recently watched content |
| Popular in Your Area | Trending in user's state/city |
| Watchlist | User's saved "My List" content |
| Recommended for You | ML-based personalized picks |

**Algorithm-Based Rows**

| Row | Algorithm |
|-----|-----------|
| Trending Now | Views + engagement in last 24-48 hours |
| Top 10 | Weighted score (views, completion, ratings) |
| New Releases | Content added in last 7-30 days |
| Just Added | Content added in last 24-48 hours |
| Leaving Soon | License expiring in next 7-14 days |
| Most Watched | All-time views |

**Language-Based Rows**

| Row | Configuration |
|-----|---------------|
| Hindi Hits | Language = Hindi, sorted by popularity |
| Marathi Special | Language = Marathi |
| Gujarati Collection | Language = Gujarati |
| Odia Originals | Language = Odia |
| Bhojpuri Blockbusters | Language = Bhojpuri |
| English Originals | Language = English |
| Regional Spotlight | Rotate featured regional content |

**Genre-Based Rows**

| Row | Configuration |
|-----|---------------|
| Action & Thriller | Genre = Action, Thriller |
| Comedy Central | Genre = Comedy |
| Romance | Genre = Romance |
| Drama | Genre = Drama |
| Horror & Suspense | Genre = Horror, Suspense |
| Documentaries | Genre = Documentary |
| Reality Shows | Genre = Reality |

**Seasonal/Event Rows**

| Row | Use Case | Schedule |
|-----|----------|----------|
| Diwali Special | Festival content | Oct-Nov |
| Holi Hits | Colorful, festive content | March |
| Independence Day | Patriotic content | Aug 15 week |
| New Year Countdown | Party, celebration content | Dec-Jan |
| IPL Season | Cricket content | Mar-May |
| Valentine's Week | Romantic content | Feb 7-14 |
| Summer Vacation | Kids, family content | Apr-Jun |
| Navratri Nights | Garba, devotional content | Sept-Oct |
| Christmas & Winter | Holiday content | December |
| Custom Event | Admin-defined | Custom dates |

---

**Row Configuration (All Types)**

| Field | Description |
|-------|-------------|
| Row ID | Unique identifier |
| Row Title | Display title (supports multiple languages) |
| Row Title (Hindi) | Hindi translation |
| Row Title (Regional) | Marathi/Gujarati/Odia translation |
| Row Type | Select from types above |
| Data Source | Algorithm / Manual / Mixed |
| Content Count | Number of items to show (default: 10-20) |
| Position | Order on home page (drag & drop) |
| Active | Enable / Disable |
| Target Audience | All / User Segment / Subscription Plan |
| Target Region | All / Specific states |
| Target Language | All / Specific app language |
| Schedule | Always / Date range / Recurring |
| Personalized | Yes / No |
| Refresh Rate | Real-time / Hourly / Daily |

---

**Create New Row**

**Step 1: Basic Info**
- Row Title (all languages)
- Row Type (select from dropdown)
- Description (internal)

**Step 2: Content Source**

*If Manual:*
- Search and add content
- Drag to reorder
- Set max items

*If Algorithm:*
- Select algorithm type
- Configure parameters
- Set fallback content

*If Mixed:*
- Pin specific content at positions
- Fill remaining with algorithm

**Step 3: Targeting**
- Target audience (All / Segment)
- Target region
- Target subscription plan
- Target device (Mobile / TV / Web)

**Step 4: Scheduling**
- Active: Always / Scheduled
- Start date & time
- End date & time
- Recurring (Daily / Weekly / Custom)

**Step 5: Preview & Publish**
- Preview on Mobile
- Preview on TV
- Preview on Web
- Save as Draft / Publish

---

**Row Actions**

| Action | Description |
|--------|-------------|
| Create | Add new row |
| Edit | Modify row settings |
| Duplicate | Clone row with new name |
| Reorder | Drag & drop position |
| Disable | Hide row temporarily |
| Delete | Remove row |
| Preview | See how row looks |
| Analytics | View row performance |

---

**Row Analytics**

| Metric | Description |
|--------|-------------|
| Impressions | Times row was shown |
| Engagement | Clicks / Impressions |
| Play Rate | % that resulted in play |
| Scroll Depth | How far users scroll in row |
| Top Performers | Best performing content in row |

**2.9.6.2 Hero Banner Management**

**Banner Configuration**

| Field | Description |
|-------|-------------|
| Banner Image | 1920x1080 landscape |
| Mobile Image | 1080x1920 portrait |
| Title | Content title |
| Subtitle | Tagline |
| CTA Button | Play / More Info |
| Link To | Content ID |
| Position | Order (1-5) |
| Schedule | Start & end date/time |
| Target | All users / Segment |

**Banner Actions**
- Add new banner
- Edit banner
- Reorder banners
- Schedule banners
- Preview on devices

**2.9.6.3 Featured Collections**

**Collection Configuration**

| Field | Description |
|-------|-------------|
| Collection Name | Display name |
| Description | Short description |
| Cover Image | Collection thumbnail |
| Content | Ordered list of content |
| Type | Manual / Smart (rules-based) |
| Status | Active / Inactive |
| Schedule | Always / Date range |

**Smart Collection Rules**
- Genre equals X
- Language equals Y
- Release year between A-B
- Rating above N
- Trending in last X days

**2.9.6.4 Top 10 Management**

**Top 10 Lists**

| List | Scope |
|------|-------|
| Top 10 in India | National |
| Top 10 in [State] | Per state (Bihar, Gujarat, etc.) |
| Top 10 Movies | Content type |
| Top 10 Series | Content type |
| Top 10 [Genre] | Per genre |

**Top 10 Configuration**
- Algorithm weights (views, completion, engagement)
- Time window (24h, 7d)
- Manual override (pin/unpin content)
- Exclude content
- Refresh frequency

---

#### 2.9.7 Video Player Configuration

**2.9.7.1 Quality Settings**

**Video Quality Tiers by Plan**

| Plan | Max Quality | Configurable |
|------|-------------|--------------|
| Free | 480p | Yes |
| Basic | 720p | Yes |
| Standard | 1080p | Yes |
| Premium | 4K | Yes |

**Bitrate Configuration**

| Quality | Default Bitrate | Adjustable Range |
|---------|-----------------|------------------|
| 4K | 15 Mbps | 12-20 Mbps |
| 1080p | 5 Mbps | 3-8 Mbps |
| 720p | 2.5 Mbps | 1.5-4 Mbps |
| 480p | 1 Mbps | 0.5-1.5 Mbps |
| 360p | 600 Kbps | 400-800 Kbps |
| 240p | 300 Kbps | 200-400 Kbps |

**Data Saver Mode**

| Setting | Description |
|---------|-------------|
| Enable Data Saver | Global toggle |
| Data Saver Quality | Default quality when enabled (240p/360p) |
| Auto-Enable on Mobile Data | Yes/No |
| Show Data Usage | Display estimated data per video |

---

**2.9.7.2 Audio Settings**

**Audio Quality by Plan**

| Plan | Max Audio | Dolby Atmos | 5.1 Surround |
|------|-----------|-------------|--------------|
| Free | Stereo | No | No |
| Basic | Stereo | No | No |
| Standard | 5.1 | No | Yes |
| Premium | Atmos | Yes | Yes |

**Audio Configuration**

| Setting | Options |
|---------|---------|
| Default Audio Language | Hindi / English / Regional |
| Audio Language Priority | Order of preference |
| Normalize Audio Levels | Yes/No (prevent loud ads) |
| Audio Description | Enable for visually impaired |
| Stereo Bitrate | 128 Kbps default |
| 5.1 Bitrate | 384 Kbps default |
| Atmos Bitrate | 768 Kbps default |

**Audio Track Management**
- Set available audio tracks per content
- Mark default audio track
- Mark audio description track
- Set audio language labels

---

**2.9.7.3 Subtitle Settings**

**Global Subtitle Defaults**

| Setting | Options |
|---------|---------|
| Default Subtitle | Off / Auto (match audio) / Specific language |
| Subtitle Language Priority | Order of preference |
| Show CC by Default | Yes/No (for hearing impaired) |

**Subtitle Appearance Defaults**

| Setting | Options |
|---------|---------|
| Font Size | Small / Medium / Large / Extra Large |
| Font Color | White / Yellow / Green / Cyan |
| Font Family | Default / Monospace / Serif / Sans-serif |
| Background Color | Black / Semi-transparent / None |
| Background Opacity | 0% - 100% |
| Edge Style | None / Drop Shadow / Raised / Depressed / Outline |
| Edge Color | Black / White / Auto |

**Subtitle Position**

| Setting | Options |
|---------|---------|
| Vertical Position | Bottom / Top / Custom % |
| Safe Area Margin | Pixels from edge |
| Avoid Overlap | Move subs when controls visible |

**Subtitle File Settings**
- Supported formats: WebVTT, SRT, TTML
- Auto-convert SRT to WebVTT on upload
- Max file size: 1 MB
- Character encoding: UTF-8

---

**2.9.7.4 Skip Markers Configuration**

**Default Skip Settings**

| Setting | Default | Configurable |
|---------|---------|--------------|
| Skip Intro Button | Show after 5 sec | Yes |
| Skip Intro Duration | Content-specific | Yes |
| Skip Recap Button | Show at start | Yes |
| Skip Recap Duration | Content-specific | Yes |
| Credits Detection | Auto at 95% completion | Yes |
| Next Episode Countdown | 5 seconds | Yes |
| Skip Credits Button | Show at credits start | Yes |

**Per-Content Skip Markers**
- Set intro start/end timestamps (mm:ss)
- Set recap start/end timestamps (mm:ss)
- Set credits start timestamp (mm:ss)
- Override defaults per content
- Bulk import skip markers via CSV

**Auto-Detection Settings**

| Setting | Description |
|---------|-------------|
| Auto-detect Intro | AI-based intro detection |
| Auto-detect Credits | Black screen + text detection |
| Detection Confidence | Minimum confidence % |
| Manual Review Queue | Low-confidence detections |

---

**2.9.7.5 Playback Rules**

**General Playback Settings**

| Rule | Configuration |
|------|---------------|
| Auto-play Next Episode | Enable/Disable by plan |
| Auto-play Preview | On hover (Web/TV) |
| Binge Watching Prompt | After X episodes (default: 3) |
| "Still Watching?" Timeout | After X hours of continuous play |
| Remember Playback Position | Yes/No |
| Remember Quality Selection | Yes/No |
| Resume Threshold | Min % to save position (default: 2%) |
| Completion Threshold | % to mark as watched (default: 90%) |

**Playback Speed Settings**

| Setting | Options |
|---------|---------|
| Enable Playback Speed | Yes/No per plan |
| Available Speeds | 0.5x, 0.75x, 1x, 1.25x, 1.5x, 1.75x, 2x |
| Default Speed | 1x |
| Remember Speed | Per session / Per content / Never |
| Audio Pitch Correction | Yes/No (maintain pitch at speed) |

**Playback Speed by Plan**

| Plan | Speed Control |
|------|---------------|
| Free | Disabled |
| Basic | 0.5x - 1.5x |
| Standard | 0.5x - 2x |
| Premium | 0.5x - 2x |

**Picture-in-Picture (PiP) Settings**

| Setting | Options |
|---------|---------|
| Enable PiP | Yes/No per platform |
| PiP on App Switch | Auto-enable when leaving app |
| PiP Size | Small / Medium / Large |
| PiP Position | Corner preference |

**Background Playback**

| Setting | Options |
|---------|---------|
| Audio-only Background | Enable/Disable |
| Continue on Screen Lock | Yes/No |
| Background for Music Videos | Yes/No |

---

**2.9.7.6 Parental Controls Configuration**

**Content Rating System**

| Rating | Age | Description |
|--------|-----|-------------|
| U | All | Universal, suitable for all |
| U/A 7+ | 7+ | Parental guidance for under 7 |
| U/A 13+ | 13+ | Parental guidance for under 13 |
| U/A 16+ | 16+ | Parental guidance for under 16 |
| A | 18+ | Adults only |

**Parental Control Settings**

| Setting | Options |
|---------|---------|
| Enable Parental Controls | Global toggle |
| Default Restriction Level | U / U/A 7+ / U/A 13+ / U/A 16+ / A |
| PIN Requirement | 4-digit / 6-digit |
| PIN for Restricted Content | Required / Optional |
| PIN Timeout | Always ask / Remember for X minutes |
| PIN Reset Method | Email / Phone OTP |

**Kids Profile Settings**

| Setting | Options |
|---------|---------|
| Kids Profile Max Rating | U / U/A 7+ |
| Disable Search in Kids | Yes/No |
| Disable Leaving Kids Profile | Require PIN |
| Kids Profile Timeout | Auto-switch after inactivity |
| Allowed Content | Whitelist specific content |
| Blocked Content | Blacklist specific content |

**Mature Content Handling**

| Setting | Options |
|---------|---------|
| Show Mature Content Warning | Yes/No |
| Blur Thumbnails | For A-rated content |
| Hide from Search | A-rated unless PIN entered |
| Restrict Time | Block A-rated during hours (e.g., 6AM-10PM) |

---

**2.9.7.7 Ad Playback Settings**

**Ad Configuration by Plan**

| Plan | Pre-roll | Mid-roll | Post-roll | Banners |
|------|----------|----------|-----------|---------|
| Free | Yes | Yes | Yes | Yes |
| Basic | Yes | No | No | No |
| Standard | No | No | No | No |
| Premium | No | No | No | No |

**Pre-roll Ad Settings**

| Setting | Options |
|---------|---------|
| Enable Pre-roll | Yes/No |
| Max Pre-roll Duration | 15s / 30s / 60s |
| Number of Pre-roll Ads | 1 / 2 / 3 |
| Skip After | 5s / 10s / Non-skippable |
| Skip Button Text | "Skip Ad" / "Skip in Xs" |

**Mid-roll Ad Settings**

| Setting | Options |
|---------|---------|
| Enable Mid-roll | Yes/No |
| First Mid-roll After | X minutes into content |
| Mid-roll Interval | Every X minutes |
| Max Mid-roll Duration | 15s / 30s |
| Number of Ads per Break | 1 / 2 |
| Skip After | 5s / 10s / Non-skippable |
| No Ads in Last | X minutes of content |
| Natural Break Points | Insert at scene changes |

**Post-roll Ad Settings**

| Setting | Options |
|---------|---------|
| Enable Post-roll | Yes/No |
| Post-roll Duration | 15s / 30s |
| Skippable | Yes / No |
| Show Before Next Episode | Yes/No |

**Ad Experience Settings**

| Setting | Options |
|---------|---------|
| Ad Volume | Match content / Normalized / Reduced |
| Ad Progress Bar | Show / Hide |
| Ad Counter | "Ad 1 of 2" display |
| Ad Countdown | Show remaining time |
| Companion Banners | Show clickable banner with video ad |

**Ad Frequency Caps**

| Setting | Options |
|---------|---------|
| Max Ads per Hour | Number limit |
| Max Ads per Session | Number limit |
| Max Same Ad Repeat | Per session / Per day |
| Ad-free After Purchase | Grace period after upgrade |

---

**2.9.7.8 DRM & Security Configuration**

**DRM Settings**

| Setting | Description |
|---------|-------------|
| DRM Provider | Bunny DRM / Widevine / FairPlay |
| License Server URL | DRM license endpoint |
| License Duration (Streaming) | 24 hours default |
| License Duration (Download) | 48 hours default |
| License Renewal | Auto-renew before expiry |
| Offline Playback Window | Max days for downloaded content |

**Security Level Requirements**

| Quality | Widevine Level | HDCP Required |
|---------|----------------|---------------|
| 4K | L1 only | HDCP 2.2 |
| 1080p | L1 only | HDCP 1.4 |
| 720p | L1 / L3 | HDCP 1.4 |
| 480p | L1 / L3 | None |
| 360p | L1 / L3 | None |

**Screen Capture Prevention**

| Setting | Options |
|---------|---------|
| Block Screenshots | Yes/No |
| Block Screen Recording | Yes/No |
| Block HDMI Capture | Yes/No |
| Block Mirroring (non-Cast) | Yes/No |
| Watermarking | Invisible user ID watermark |
| Secure Window Flag | Android FLAG_SECURE |

**Device Security**

| Setting | Options |
|---------|---------|
| Root/Jailbreak Detection | Block / Warn / Allow |
| Emulator Detection | Block / Allow |
| VPN Detection | Block / Warn / Allow |
| Device Binding | Bind license to device |
| Max Devices per Account | Per plan configuration |

**Content Protection Logging**

| Event | Logged |
|-------|--------|
| Screenshot Attempt | Yes |
| Screen Recording Attempt | Yes |
| HDMI Connection | Yes |
| DRM License Request | Yes |
| Playback on Rooted Device | Yes |

---

**2.9.7.9 Player UI Configuration**

**Control Bar Settings**

| Setting | Options |
|---------|---------|
| Control Bar Timeout | Seconds before auto-hide |
| Control Bar Style | Minimal / Standard / Full |
| Progress Bar Style | Line / Thick / With thumbnails |
| Show Preview Thumbnails | Yes/No |
| Thumbnail Interval | Every X seconds |

**Player Branding**

| Setting | Options |
|---------|---------|
| Show Logo | Yes/No |
| Logo Position | Top-left / Top-right |
| Logo Opacity | 0% - 100% |
| Loading Spinner | Default / Custom |
| Loading Animation | Spinner / Logo animation |

**Gesture Controls (Mobile)**

| Gesture | Action | Configurable |
|---------|--------|--------------|
| Double-tap Left | Rewind 10s | Yes (5s/10s/15s) |
| Double-tap Right | Forward 10s | Yes (5s/10s/15s) |
| Swipe Up/Down Left | Brightness | Enable/Disable |
| Swipe Up/Down Right | Volume | Enable/Disable |
| Swipe Left/Right | Seek | Enable/Disable |
| Pinch | Zoom/Fit | Enable/Disable |
| Long Press | 2x Speed | Enable/Disable |

**TV Remote Controls**

| Button | Action | Configurable |
|--------|--------|--------------|
| OK/Select | Play/Pause | No |
| Left/Right | Seek ±10s | Yes (5s/10s/30s) |
| Up/Down | Volume / Menu | Configurable |
| Back | Exit / Show Info | Configurable |
| Play/Pause | Toggle playback | No |
| Info | Show details | Yes |

---

#### 2.9.8 Search & Discovery Configuration

**2.9.8.1 Search Settings**

**Search Behavior**

| Setting | Options |
|---------|---------|
| Minimum Query Length | 1-3 characters |
| Autocomplete Delay | 100-500ms |
| Results per Page | 10-50 |
| Search History | Enable/Disable |
| Safe Search | On/Off/Strict |

**Search Weights**

| Field | Default Weight | Adjustable |
|-------|----------------|------------|
| Title | 10 | Yes |
| Original Title | 8 | Yes |
| Cast | 6 | Yes |
| Director | 5 | Yes |
| Description | 3 | Yes |
| Tags | 4 | Yes |

**2.9.8.2 Search Synonyms**

**Synonym Management**
- Add synonym pairs (e.g., "SRK" → "Shah Rukh Khan")
- Bulk import synonyms
- Language-specific synonyms
- Transliteration mappings

**Common Synonyms**

| Term | Synonyms |
|------|----------|
| SRK | Shah Rukh Khan, Shahrukh |
| Big B | Amitabh Bachchan |
| Action | Thriller, Fight |

**2.9.8.3 Search Boosting**

**Boost Rules**

| Rule Type | Configuration |
|-----------|---------------|
| New Content | Boost for X days |
| Trending | Boost by popularity |
| Sponsored | Manual boost factor |
| Seasonal | Date-based boost |

**Bury Rules**
- Hide specific content from search
- Demote low-quality content
- Age-restrict content

**2.9.8.4 Search Analytics**

| Metric | Description |
|--------|-------------|
| Top Searches | Most searched terms |
| Zero Results | Searches with no results |
| Search Success | % leading to play |
| Trending Searches | Rising search terms |

---

#### 2.9.9 Recommendations Configuration

**2.9.9.1 Algorithm Settings**

**Recommendation Sources**

| Source | Weight | Adjustable |
|--------|--------|------------|
| Watch History | 30% | Yes |
| Similar Content | 25% | Yes |
| Trending | 20% | Yes |
| Editorial | 15% | Yes |
| New Releases | 10% | Yes |

**Algorithm Parameters**

| Parameter | Description | Default |
|-----------|-------------|---------|
| History Window | Days of history to use | 30 |
| Similarity Threshold | Min similarity score | 0.6 |
| Diversity Factor | Avoid repetition | 0.3 |
| Exploration Rate | New content exposure | 0.1 |

**2.9.9.2 "Because You Watched" Rules**

| Rule | Configuration |
|------|---------------|
| Minimum Watch % | 25% to trigger |
| Max Recommendations | 10 per source |
| Exclude Watched | Yes/No |
| Genre Match | Required/Preferred |

**2.9.9.3 Editorial Picks**

**Create Editorial Collection**
- Title (e.g., "Staff Picks This Week")
- Description
- Select content manually
- Set display position
- Schedule dates
- Target audience

**2.9.9.4 A/B Testing**

**Create Experiment**
- Experiment name
- Variant A (control)
- Variant B (test)
- Traffic split (%)
- Success metric
- Duration
- Auto-winner selection

---

#### 2.9.10 Notification Management

**2.9.10.1 Notification Channels**

| Channel | Provider | Use Case |
|---------|----------|----------|
| Push | Firebase Cloud Messaging | Real-time alerts, reminders |
| Email | AWS SES | Transactional, marketing |
| SMS | MSG91 | OTP, critical alerts |
| WhatsApp | WhatsApp Business API | Promotional, reminders |
| In-App | Native | Notification center |

**Channel Configuration**

| Channel | Settings |
|---------|----------|
| Push | Enable/Disable, Sound, Vibration, Priority |
| Email | From name, From email, Reply-to, Footer |
| SMS | Sender ID, Template DLT registration |
| WhatsApp | Business number, Template approval |
| In-App | Badge count, Sound, Auto-dismiss |

---

**2.9.10.2 Notification Types**

| Type | Channels | Trigger |
|------|----------|---------|
| New Release | Push, Email, WhatsApp, In-App | Content published |
| Reminder | Push, WhatsApp, In-App | Incomplete content |
| Subscription | Email, SMS, WhatsApp | Payment events |
| Promotional | Push, Email, WhatsApp, In-App | Campaign |
| System | Push, In-App | Maintenance, updates |
| Transactional | Email, SMS | OTP, receipts |
| Social | Push, In-App | Watch party, follows |

---

**2.9.10.3 In-App Notification Center**

**Notification Center UI (Bell Icon)**

| Feature | Description |
|---------|-------------|
| Bell Icon | Shows in app header |
| Unread Badge | Count of unread notifications |
| Notification List | Scrollable list of notifications |
| Mark as Read | Individual or all |
| Delete | Remove notification |
| Deep Link | Tap to navigate |

**In-App Notification Types**

| Type | Icon | Action |
|------|------|--------|
| New Content | Play icon | Go to content |
| Subscription | Card icon | Go to subscription |
| Promo | Gift icon | Go to offer |
| System | Info icon | Show message |
| Social | User icon | Go to activity |

**In-App Notification Settings**

| Setting | Options |
|---------|---------|
| Max Stored | 50-100 notifications |
| Auto-Delete After | 30 days |
| Group Similar | Yes/No |
| Show Timestamp | Relative (2h ago) / Absolute |
| Enable Animations | Yes/No |

---

**2.9.10.4 Create Notification**

**Step 1: Basic Information**

| Field | Description |
|-------|-------------|
| Notification Name | Internal reference name |
| Title | Display title (50 chars) |
| Body | Message content (150 chars) |
| Image | Rich notification image (optional) |
| Icon | Custom icon (optional) |

**Step 2: Channel Selection**

| Channel | Options |
|---------|---------|
| Push | Enable + Priority (High/Normal/Low) |
| Email | Enable + Template selection |
| SMS | Enable + Template (DLT approved) |
| WhatsApp | Enable + Template (approved) |
| In-App | Enable + Category |

**Step 3: Targeting**

| Field | Options |
|-------|---------|
| Target Audience | All / Segment / Individual |
| User Segment | Select saved segment |
| Subscription Plan | All / Specific plans |
| Region | All / Specific states |
| Language | All / Specific languages |
| Device | All / Android / iOS / Web |
| Last Active | Within X days |

**Step 4: Action & Deep Link**

| Field | Options |
|-------|---------|
| Action Type | Open App / Deep Link / URL / None |
| Deep Link | Screen path (e.g., /content/123) |
| External URL | Web URL (for email) |
| CTA Button | Button text (email/WhatsApp) |

**Step 5: Scheduling**

| Field | Options |
|-------|---------|
| Send Time | Immediate / Scheduled |
| Schedule Date | Date picker |
| Schedule Time | Time picker |
| Timezone | User's timezone / Fixed |
| Expiry | Auto-expire after X hours |
| Best Time | AI-optimized send time |

**Step 6: Preview & Send**
- Preview on Mobile
- Preview on Email
- Preview on WhatsApp
- Send Test
- Save as Draft
- Schedule / Send Now

---

**2.9.10.5 Notification Templates**

**Template Categories**

| Category | Templates |
|----------|-----------|
| Onboarding | Welcome, Complete profile, First watch |
| Content | New release, New season, Recommendations |
| Engagement | Continue watching, Watchlist reminder, Trending |
| Subscription | Renewal, Expiry warning, Payment failed, Upgraded |
| Transactional | OTP, Receipt, Refund confirmation |
| Promotional | Sale, Promo code, Special offer |
| Social | Watch party invite, Friend joined |
| Re-engagement | We miss you, Come back offer |
| Lifecycle | Birthday, Anniversary, Milestone |

**Template Configuration**

| Field | Description |
|-------|-------------|
| Template Name | Internal name |
| Template ID | Unique identifier |
| Category | Select from above |
| Channel | Push / Email / SMS / WhatsApp / All |
| Subject (Email) | Email subject line |
| Title | Notification title |
| Body | Message body |
| Image | Default image |
| CTA | Call-to-action button |
| Variables | Dynamic placeholders |

**Template Variables**

| Variable | Description |
|----------|-------------|
| `{{user.name}}` | User's first name |
| `{{user.email}}` | User's email |
| `{{content.title}}` | Content title |
| `{{content.image}}` | Content thumbnail URL |
| `{{content.url}}` | Deep link to content |
| `{{subscription.plan}}` | Current plan name |
| `{{subscription.expiry}}` | Expiry date |
| `{{subscription.days_left}}` | Days until expiry |
| `{{promo.code}}` | Promo code |
| `{{promo.discount}}` | Discount value |
| `{{user.watch_progress}}` | % watched |
| `{{user.last_watched}}` | Last watched title |

---

**2.9.10.6 Automated Triggers**

**Onboarding Triggers**

| Trigger | Timing | Notification |
|---------|--------|--------------|
| Welcome | On signup | "Welcome to MahuaPlay!" |
| Complete Profile | 1 day after signup | "Complete your profile" |
| First Watch | If no watch in 2 days | "Start watching now" |
| Add Watchlist | If empty watchlist | "Save content for later" |

**Engagement Triggers**

| Trigger | Timing | Notification |
|---------|--------|--------------|
| Continue Watching | 1 day after pause | "Continue [Title]" |
| Watchlist Reminder | Weekly | "Items in your watchlist" |
| New Episode | On publish | "[Show] new episode out!" |
| New Season | On publish | "[Show] Season X is here!" |
| Trending Alert | Daily | "Trending: [Title]" |
| Recommendations | Weekly | "Picks for you" |

**Subscription Triggers**

| Trigger | Timing | Notification |
|---------|--------|--------------|
| Trial Ending | 2 days before | "Trial ends in 2 days" |
| Subscription Expiry | 7, 3, 1 days before | "Renew your subscription" |
| Payment Failed | On failure | "Payment failed" |
| Payment Retry | 1, 3, 5 days after fail | "Update payment method" |
| Subscription Renewed | On success | "Subscription renewed" |
| Plan Upgraded | On upgrade | "Welcome to [Plan]!" |
| Plan Downgraded | On downgrade | "Plan changed" |

**Re-engagement Triggers**

| Trigger | Timing | Notification |
|---------|--------|--------------|
| Inactive 7 days | After 7 days | "We miss you!" |
| Inactive 14 days | After 14 days | "Come back, here's what's new" |
| Inactive 30 days | After 30 days | "Special offer to return" |
| Churned User | After cancellation | "We'd love you back" |
| Win-back | 60 days after churn | "Exclusive return offer" |

**Lifecycle Triggers**

| Trigger | Timing | Notification |
|---------|--------|--------------|
| Birthday | On birthday | "Happy Birthday! Here's a gift" |
| Signup Anniversary | Yearly | "Thanks for 1 year with us!" |
| Watch Milestone | On milestone | "You've watched 100 hours!" |
| First Month | 30 days after signup | "Your first month recap" |

**Content Triggers**

| Trigger | Timing | Notification |
|---------|--------|--------------|
| Followed Show Update | On new content | "New from [Show]" |
| Actor New Release | On publish | "New movie with [Actor]" |
| Genre Recommendation | Weekly | "New in [Favorite Genre]" |
| Leaving Soon | 7 days before | "[Title] leaving soon" |

**Social Triggers**

| Trigger | Timing | Notification |
|---------|--------|--------------|
| Watch Party Invite | On invite | "[User] invited you" |
| Friend Joined | On signup | "[Friend] joined MahuaPlay" |
| Review Reply | On reply | "Someone replied to your review" |

---

**2.9.10.7 Campaign Management**

**Create Campaign**

| Field | Description |
|-------|-------------|
| Campaign Name | Internal name |
| Objective | Awareness / Engagement / Conversion / Retention |
| Start Date | Campaign start |
| End Date | Campaign end |
| Target Audience | Segment selection |
| Budget | Notification budget cap |

**Campaign Types**

| Type | Description |
|------|-------------|
| One-time | Single notification blast |
| Drip | Sequence over days |
| Triggered | Based on user action |
| Recurring | Daily/Weekly/Monthly |
| A/B Test | Test variants |

**Drip Campaign Sequence**

| Day | Notification |
|-----|--------------|
| Day 0 | Welcome message |
| Day 1 | Feature highlight |
| Day 3 | Content recommendation |
| Day 7 | Engagement prompt |
| Day 14 | Special offer |

**Campaign Analytics**

| Metric | Description |
|--------|-------------|
| Sent | Total notifications sent |
| Delivered | Successfully delivered |
| Delivery Rate | Delivered / Sent % |
| Opened | Push opens / Email opens |
| Open Rate | Opened / Delivered % |
| Clicked | CTA / Deep link clicks |
| Click Rate | Clicked / Opened % |
| Converted | Target action completed |
| Conversion Rate | Converted / Clicked % |
| Unsubscribed | Opted out |
| Revenue | Revenue attributed |

---

**2.9.10.8 Notification Settings**

**Global Settings**

| Setting | Options |
|---------|---------|
| Quiet Hours Start | 10 PM (default) |
| Quiet Hours End | 8 AM (default) |
| Respect User Timezone | Yes/No |
| Max Push per Day | 3-5 (configurable) |
| Max Email per Week | 5-10 (configurable) |
| Max SMS per Month | 5 (configurable) |
| Max WhatsApp per Week | 3 (configurable) |
| Frequency Cap | 1 per content per week |

**Channel-Specific Settings**

| Channel | Settings |
|---------|----------|
| Push | Sound, Vibration, LED color, Priority |
| Email | Unsubscribe link (required), Footer, Logo |
| SMS | Opt-out instructions, Sender ID |
| WhatsApp | Business verified, Template compliance |

**User Preference Settings**

| Preference | Options |
|------------|---------|
| Allow Users to Disable | Per channel toggle |
| Preference Center | Web page for settings |
| One-click Unsubscribe | Email footer link |
| Granular Preferences | By notification type |

---

**2.9.10.9 Notification Analytics Dashboard**

**Overview Metrics**

| Metric | Description |
|--------|-------------|
| Total Sent | All notifications sent |
| Delivery Rate | Overall delivery % |
| Engagement Rate | Opens + Clicks % |
| Opt-out Rate | Unsubscribe % |

**Channel Performance**

| Channel | Metrics |
|---------|---------|
| Push | Sent, Delivered, Opened, CTR |
| Email | Sent, Delivered, Opened, Clicked, Bounced |
| SMS | Sent, Delivered, Failed |
| WhatsApp | Sent, Delivered, Read, Replied |
| In-App | Displayed, Clicked, Dismissed |

**Reports**

| Report | Description |
|--------|-------------|
| Daily Summary | Daily notification metrics |
| Campaign Performance | Per-campaign analysis |
| Template Performance | Best performing templates |
| Best Send Time | Optimal send times by segment |
| Engagement Trends | Weekly/Monthly trends |

---

#### 2.9.11 Advertisement Management

**2.9.11.1 Ad Network Integration**

**Supported Ad Networks**

| Network | Type | Use Case |
|---------|------|----------|
| Google Ad Manager (GAM) | Primary | Programmatic + Direct |
| Meta Audience Network | Secondary | Mobile display |
| Amazon Ads | Secondary | Fire TV, Echo |
| InMobi | Regional | India-focused mobile |
| Verizon Media | Programmatic | Video ads |
| SpotX | Video SSP | Premium video |
| PubMatic | SSP | Programmatic |
| IronSource | Mediation | Ad mediation |
| Unity Ads | Gaming | Gamified content |
| Custom/Direct | Direct Sales | Brand partnerships |

**Ad Network Configuration**

| Setting | Description |
|---------|-------------|
| Network ID | Account/Publisher ID |
| API Keys | Authentication keys |
| Ad Unit IDs | Per placement IDs |
| Refresh Rate | For display ads |
| Timeout | Max load time (ms) |
| Priority | Network priority order |
| Fill Rate Threshold | Minimum fill % |
| Floor Price | Minimum CPM |

**Mediation & Waterfall**

| Setting | Description |
|---------|-------------|
| Mediation Type | Waterfall / Bidding / Hybrid |
| Network Order | Priority sequence |
| Auto-optimize | Yes/No (ML-based) |
| eCPM Floors | Per network minimums |
| Refresh Interval | Seconds between requests |

---

**2.9.11.2 Ad Formats**

**Video Ad Formats**

| Format | Placement | Duration | Skippable |
|--------|-----------|----------|-----------|
| Pre-roll | Before content | 6s / 15s / 30s | Configurable |
| Mid-roll | During content | 15s / 30s | Configurable |
| Post-roll | After content | 15s / 30s | Yes |
| Bumper | Before content | 6s | No |
| Outstream | In-feed / Article | 15-30s | Yes |

**Display Ad Formats**

| Format | Size | Placement |
|--------|------|-----------|
| Banner | 320x50, 728x90 | Top/Bottom of screen |
| Medium Rectangle | 300x250 | In-feed, Browse |
| Leaderboard | 728x90 | Header |
| Interstitial | Full screen | Between content |
| Native | Variable | In-feed |
| Sticky Banner | 320x50 | Fixed bottom |

**Interactive Ad Formats**

| Format | Description | Use Case |
|--------|-------------|----------|
| Shoppable Ads | Click to buy products | E-commerce |
| Poll Ads | Interactive voting | Engagement |
| Quiz Ads | Trivia with rewards | Brand recall |
| Playable Ads | Mini-game experience | Gaming |
| AR Ads | Augmented reality | Premium brands |
| 360° Video Ads | Immersive video | Automotive, Travel |
| Choose Your Ad | User selects ad type | User preference |

**OTT-Specific Ad Formats**

| Format | Description | Platform |
|--------|-------------|----------|
| Pause Ads | Display when paused | All |
| Binge Ads | Between episodes | All |
| Overlay Ads | L-band / Lower third | TV |
| Squeeze-back | Content shrinks | TV |
| Dynamic Overlay | Logo/text on content | TV |
| T-Commerce | Shop via remote | TV |
| QR Code Ads | Scan to interact | TV |
| Second Screen | Sync with mobile | TV |

**Sponsored Content Formats**

| Format | Description |
|--------|-------------|
| Sponsored Row | Branded content row |
| Sponsored Card | Featured content card |
| Branded Tile | Logo on content tile |
| Sponsored Search | Promoted search results |
| Branded Channel | Brand's content hub |
| Content Integration | In-content placement |

---

**2.9.11.3 Ad Placement Configuration**

**Video Ad Placements**

| Placement | Configuration |
|-----------|---------------|
| Pre-roll Position | Before / After trailer |
| Pre-roll Max Ads | 1-3 ads |
| Pre-roll Max Duration | 30s / 60s / 90s |
| Mid-roll First Break | After X minutes |
| Mid-roll Interval | Every Y minutes |
| Mid-roll Max Ads | 1-2 per break |
| Mid-roll Max Duration | 30s / 60s |
| Post-roll Enabled | Yes/No |
| Post-roll Before Next | Yes/No |

**Ad Break Rules**

| Rule | Configuration |
|------|---------------|
| Minimum Content Length | Show ads only if > X min |
| No Ads in First | X minutes |
| No Ads in Last | X minutes |
| Natural Break Points | Insert at scene changes |
| Ad Pods | Group multiple ads |
| Pod Max Duration | Total pod length |
| Competitive Separation | Minutes between competitors |
| Frequency Cap | Max same ad per session |

**Display Ad Placements**

| Placement | Location | Trigger |
|-----------|----------|---------|
| Home Banner | Top of home | On load |
| Browse Banner | Category pages | On load |
| Search Banner | Search results | On search |
| Pause Ad | On video pause | On pause |
| Exit Interstitial | On app exit | On exit intent |
| Splash Ad | App launch | Every X launches |
| Content Interstitial | Between episodes | On episode end |

**Plan-Based Ad Configuration**

| Plan | Pre-roll | Mid-roll | Post-roll | Display | Pause Ads |
|------|----------|----------|-----------|---------|-----------|
| Free | 2 ads | Every 15 min | Yes | Yes | Yes |
| Basic | 1 ad | None | No | No | No |
| Standard | None | None | None | None | None |
| Premium | None | None | None | None | None |

**Ad-Lite Options**

| Option | Description |
|--------|-------------|
| Reduced Ad Plan | 50% fewer ads at lower price |
| Ad-free Weekends | No ads on Sat-Sun |
| Ad-free Movies | No mid-roll in movies |
| Ad-free Binge | No ads after 3rd episode |

---

**2.9.11.4 Ad Targeting**

**Demographic Targeting**

| Parameter | Options |
|-----------|---------|
| Age Range | 13-17, 18-24, 25-34, 35-44, 45-54, 55+ |
| Gender | Male, Female, All |
| Household Income | Low, Medium, High, Premium |
| Education | School, Graduate, Post-graduate |
| Marital Status | Single, Married, Family |
| Parental Status | No kids, Parents |

**Geographic Targeting**

| Level | Options |
|-------|---------|
| Country | India (primary) |
| State | Bihar, Gujarat, Maharashtra, Odisha, etc. |
| City | Tier 1, Tier 2, Tier 3 cities |
| PIN Code | Specific areas |
| Radius | Around location |
| Urban/Rural | Urban, Semi-urban, Rural |

**Behavioral Targeting**

| Behavior | Description |
|----------|-------------|
| Watch History | Based on genres watched |
| Binge Watchers | >3 episodes per session |
| Movie Lovers | Prefer movies over series |
| New Users | Registered < 30 days |
| Churned Users | Subscription cancelled |
| High Engagers | >2 hours daily watch time |
| Weekend Watchers | Active on weekends |
| Night Owls | Watch after 10 PM |

**Content Targeting**

| Parameter | Options |
|-----------|---------|
| Genre | Action, Comedy, Drama, Horror, etc. |
| Language | Hindi, Marathi, Gujarati, Odia, etc. |
| Content Rating | U, U/A, A |
| Content Type | Movie, Series, Short, Live |
| Release Type | New release, Library |
| Mood | Happy, Thrilling, Romantic |

**Device & Technical Targeting**

| Parameter | Options |
|-----------|---------|
| Device Type | Mobile, Tablet, TV, Web |
| OS | Android, iOS, Fire OS, Tizen |
| Connection | WiFi, 4G, 5G, 3G |
| Device Make | Samsung, Xiaomi, Apple, etc. |
| Screen Size | Small, Medium, Large |
| App Version | Specific versions |

**Time-Based Targeting**

| Parameter | Options |
|-----------|---------|
| Day of Week | Mon-Sun, Weekday, Weekend |
| Time of Day | Morning, Afternoon, Evening, Night |
| Day Part | Prime time, Late night |
| Season | Summer, Winter, Monsoon |
| Festivals | Diwali, Holi, Eid, etc. |
| Events | IPL, Elections, etc. |

**Contextual Targeting**

| Parameter | Options |
|-----------|---------|
| Content Mood | Match ad to content mood |
| Scene Type | Action, Romance, Comedy scene |
| Brand Safety | Avoid sensitive content |
| Competitor Content | Avoid/Target competitor content |

**Custom Audiences**

| Type | Description |
|------|-------------|
| Lookalike | Similar to existing customers |
| Retargeting | Users who saw ad before |
| CRM Upload | Advertiser's customer list |
| Segment Sync | Third-party data |
| Intent | In-market for category |

---

**2.9.11.5 Ad Campaign Management**

**Create Ad Campaign**

**Step 1: Campaign Details**

| Field | Description |
|-------|-------------|
| Campaign Name | Internal reference |
| Advertiser | Select advertiser |
| Brand | Brand name |
| Campaign Type | Awareness / Traffic / Conversion |
| Objective | Views / Clicks / Completions |
| Start Date | Campaign start |
| End Date | Campaign end |
| Budget | Total budget (INR) |
| Budget Type | Daily / Lifetime |
| Pacing | Even / Accelerated |

**Step 2: Ad Creatives**

| Field | Description |
|-------|-------------|
| Creative Name | Asset name |
| Format | Video / Display / Native |
| File Upload | Video/Image file |
| Duration | For video ads |
| Companion Banner | Optional display |
| Click URL | Landing page |
| Tracking Pixels | Third-party tracking |
| VAST/VPAID | Ad tag (if external) |

**Creative Specifications**

| Format | Specs |
|--------|-------|
| Video (Pre/Mid-roll) | MP4, H.264, 1080p, 15-30s, <50MB |
| Video (Bumper) | MP4, H.264, 1080p, 6s, <10MB |
| Banner (Mobile) | JPG/PNG, 320x50, <50KB |
| Banner (Desktop) | JPG/PNG, 728x90, <100KB |
| Interstitial | JPG/PNG, 1080x1920, <200KB |
| Native | Image + Title + Description |

**Step 3: Targeting**
- Select targeting parameters (as defined above)
- Create target audience
- Estimate reach

**Step 4: Placements**

| Field | Options |
|-------|---------|
| Placement Type | Automatic / Manual |
| Video Placements | Pre-roll, Mid-roll, Post-roll |
| Display Placements | Home, Browse, Pause |
| Device Targeting | Mobile, TV, Web |
| Content Exclusions | Genres/Ratings to avoid |

**Step 5: Budget & Bidding**

| Field | Options |
|-------|---------|
| Pricing Model | CPM / CPCV / CPC / CPV |
| Bid Amount | INR per unit |
| Bid Strategy | Manual / Auto-optimize |
| Frequency Cap | Max impressions per user |
| Day Parting | Specific hours |

**Pricing Models**

| Model | Description | Rate |
|-------|-------------|------|
| CPM | Cost per 1000 impressions | ₹50-500 |
| CPCV | Cost per completed view | ₹1-10 |
| CPC | Cost per click | ₹2-20 |
| CPV | Cost per view (2 sec) | ₹0.50-5 |
| Flat Fee | Fixed daily/weekly rate | Custom |
| Revenue Share | % of ad revenue | 30-70% |

**Step 6: Review & Launch**
- Preview campaign
- Review settings
- Submit for approval
- Launch / Schedule

---

**2.9.11.6 Campaign List & Management**

**Campaign List View**

| Column | Description |
|--------|-------------|
| Campaign Name | Campaign title |
| Advertiser | Advertiser name |
| Status | Draft / Pending / Active / Paused / Completed |
| Budget | Total / Spent |
| Impressions | Delivered |
| Completion Rate | For video |
| CTR | Click-through rate |
| Dates | Start - End |
| Actions | Edit, Pause, Clone, Report |

**Campaign Actions**

| Action | Description |
|--------|-------------|
| Edit | Modify campaign settings |
| Pause | Temporarily stop |
| Resume | Restart paused campaign |
| Clone | Duplicate campaign |
| Archive | Move to archive |
| Delete | Remove permanently |
| Export | Download report |

**Campaign Optimization**

| Feature | Description |
|---------|-------------|
| Auto-optimization | ML-based budget allocation |
| A/B Testing | Test creatives/targeting |
| Daypart Optimization | Best performing hours |
| Placement Optimization | Best performing slots |
| Creative Rotation | Rotate multiple creatives |
| Frequency Optimization | Optimal frequency cap |

---

**2.9.11.7 Ad Inventory Management**

**Inventory Overview**

| Metric | Description |
|--------|-------------|
| Total Impressions | Available monthly |
| Sold Impressions | Booked inventory |
| Available | Remaining inventory |
| Fill Rate | % filled |
| Sell-Through Rate | % sold vs available |

**Inventory by Placement**

| Placement | Available | Sold | Fill Rate |
|-----------|-----------|------|-----------|
| Pre-roll | 10M | 7M | 70% |
| Mid-roll | 15M | 10M | 67% |
| Home Banner | 5M | 4M | 80% |
| Pause Ads | 3M | 1M | 33% |

**Inventory Forecasting**

| Feature | Description |
|---------|-------------|
| Availability Check | Check inventory for dates |
| Forecast | Predict future inventory |
| Contention | Competing campaigns |
| Recommendations | Optimal booking |

**Direct vs Programmatic Split**

| Type | Allocation |
|------|------------|
| Direct Sales | 60% (premium, guaranteed) |
| Programmatic | 40% (real-time bidding) |

---

**2.9.11.8 Advertiser Management**

**Advertiser Account**

| Field | Description |
|-------|-------------|
| Company Name | Advertiser name |
| Industry | Category |
| Contact Person | Primary contact |
| Email | Contact email |
| Phone | Contact phone |
| Address | Billing address |
| GST Number | Tax ID |
| Payment Terms | Net 30 / Prepaid |
| Credit Limit | Max outstanding |
| Account Manager | Internal owner |

**Advertiser List**

| Column | Description |
|--------|-------------|
| Advertiser | Company name |
| Industry | Category |
| Active Campaigns | Running campaigns |
| Total Spend | Lifetime spend |
| Outstanding | Pending payment |
| Status | Active / Inactive |

**Advertiser Portal Access**

| Feature | Description |
|---------|-------------|
| Self-serve Dashboard | Advertiser login |
| Campaign Creation | Create campaigns |
| Reporting | View performance |
| Billing | View invoices |
| Creative Upload | Upload assets |
| Support | Raise tickets |

---

**2.9.11.9 Ad Revenue Management**

**Revenue Dashboard**

| Metric | Period |
|--------|--------|
| Total Revenue | Today / Week / Month / Year |
| Revenue by Network | Breakdown by ad network |
| Revenue by Format | Video / Display / Native |
| Revenue by Placement | Pre-roll / Mid-roll / Banner |
| Revenue by Advertiser | Top advertisers |

**Revenue Metrics**

| Metric | Description |
|--------|-------------|
| Gross Revenue | Total ad revenue |
| Net Revenue | After network fees |
| eCPM | Effective CPM |
| RPM | Revenue per 1000 views |
| ARPU | Ad revenue per user |
| Fill Rate | % inventory filled |

**Payment & Billing**

| Feature | Description |
|---------|-------------|
| Invoice Generation | Auto-generate invoices |
| Payment Tracking | Track payments |
| Revenue Share | Partner payouts |
| Tax Calculation | GST computation |
| Reconciliation | Match with networks |

**Revenue Reports**

| Report | Description |
|--------|-------------|
| Daily Revenue | Day-by-day breakdown |
| Advertiser Revenue | Per-advertiser spend |
| Placement Performance | Revenue by slot |
| Network Performance | Revenue by network |
| Trend Analysis | Month-over-month |

---

**2.9.11.10 Ad Quality & Brand Safety**

**Creative Review**

| Check | Description |
|-------|-------------|
| Format Compliance | Meets specs |
| Content Review | Appropriate content |
| Audio Levels | Normalized volume |
| Click URL | Valid destination |
| Malware Scan | Security check |
| Competitor Check | Not competitor |

**Review Status**

| Status | Description |
|--------|-------------|
| Pending | Awaiting review |
| Approved | Ready to serve |
| Rejected | Failed review |
| Changes Requested | Needs modification |

**Brand Safety Settings**

| Setting | Options |
|---------|---------|
| Content Categories | Block specific genres |
| Keywords | Block specific keywords |
| Sensitive Topics | Politics, Adult, Violence |
| News Content | Block/Allow |
| User-Generated | Block/Allow |
| Competitor Brands | Block list |

**Ad Quality Rules**

| Rule | Description |
|------|-------------|
| No Deceptive Ads | Misleading claims |
| No Adult Content | Explicit material |
| No Hate Speech | Offensive content |
| No Malware | Malicious code |
| Audio Standards | Max volume levels |
| Animation Limits | No flashing content |

**Blocked Categories**

| Category | Description |
|----------|-------------|
| Gambling | Betting, Casino |
| Alcohol | Liquor ads |
| Tobacco | Cigarettes, Vaping |
| Political | Election ads |
| Pharmaceuticals | Prescription drugs |
| Weapons | Firearms, Ammunition |
| Adult | 18+ content |
| Competitors | Listed brands |

---

**2.9.11.11 Ad Performance Analytics**

**Real-Time Dashboard**

| Metric | Description |
|--------|-------------|
| Active Campaigns | Currently running |
| Impressions Today | Today's delivery |
| Revenue Today | Today's earnings |
| Fill Rate | Current fill % |
| Top Performer | Best campaign |

**Campaign Analytics**

| Metric | Description |
|--------|-------------|
| Impressions | Total ad views |
| Unique Reach | Unique users |
| Frequency | Avg impressions per user |
| Viewability | % viewable impressions |
| Completion Rate | Video completion % |
| VTR | View-through rate |
| CTR | Click-through rate |
| Conversions | Post-click actions |
| Spend | Budget consumed |
| Pacing | Ahead/Behind schedule |

**Video Ad Metrics**

| Metric | Description |
|--------|-------------|
| Video Starts | Number of starts |
| 25% Viewed | Reached 25% |
| 50% Viewed | Reached 50% |
| 75% Viewed | Reached 75% |
| 100% Viewed | Completed |
| Avg View Time | Average duration |
| Skip Rate | % skipped |
| Mute Rate | % muted |

**Display Ad Metrics**

| Metric | Description |
|--------|-------------|
| Impressions | Views |
| Viewability | Above fold, >1 sec |
| Clicks | Interactions |
| CTR | Click-through rate |
| Hover Rate | Mouse hover % |
| Engagement | Time on ad |

**Attribution & Conversion**

| Model | Description |
|-------|-------------|
| Last Click | Credit to last ad |
| First Click | Credit to first ad |
| Linear | Equal credit |
| Time Decay | Recent gets more |
| View-Through | Post-view conversion |
| Cross-Device | Multi-device tracking |

---

**2.9.11.12 Programmatic Advertising**

**SSP Integration**

| Partner | Type | Status |
|---------|------|--------|
| Google AdX | Exchange | Active |
| SpotX | Video SSP | Active |
| PubMatic | SSP | Active |
| OpenX | Exchange | Active |
| Magnite | SSP | Active |

**Header Bidding**

| Setting | Configuration |
|---------|---------------|
| Enable Header Bidding | Yes/No |
| Timeout | 1000-3000ms |
| Price Granularity | Low/Medium/High/Custom |
| Bidders | Select active bidders |
| Floor Price | Minimum CPM |

**Private Marketplace (PMP)**

| Feature | Description |
|---------|-------------|
| Deal ID | Unique deal identifier |
| Deal Type | Preferred / Private Auction / Programmatic Guaranteed |
| Advertiser | Specific buyer |
| Floor Price | Deal floor |
| Inventory | Reserved inventory |
| Priority | Deal priority |

**Real-Time Bidding (RTB)**

| Setting | Configuration |
|---------|---------------|
| Enable RTB | Yes/No |
| Open Auction | Allow open bidding |
| Floor Price | Minimum bid |
| Blocked Buyers | Advertiser blacklist |
| Blocked Categories | Category restrictions |

---

**2.9.11.13 Ad Server Configuration**

**VAST/VPAID Settings**

| Setting | Description |
|---------|-------------|
| VAST Version | 3.0 / 4.0 / 4.1 / 4.2 |
| VPAID | Enable/Disable |
| Wrapper Limit | Max wrapper depth |
| Timeout | Ad load timeout |
| Fallback | Backup ad tag |

**Ad Serving Rules**

| Rule | Description |
|------|-------------|
| Geo-targeting | Serve by location |
| Device Targeting | Serve by device |
| Frequency Capping | Limit per user |
| Day Parting | Time-based serving |
| Competitive Separation | Time between competitors |
| Sequential Serving | Ordered ad serving |

**SSAI (Server-Side Ad Insertion)**

| Setting | Description |
|---------|-------------|
| Enable SSAI | Yes/No |
| Ad Stitching | Server-side stitching |
| Manifest Manipulation | Dynamic manifest |
| Beacon Tracking | Client-side pings |
| Ad Markers | SCTE-35 markers |

---

**2.9.11.14 Ad Reports**

**Standard Reports**

| Report | Description |
|--------|-------------|
| Delivery Report | Impressions, reach, frequency |
| Revenue Report | Revenue by various dimensions |
| Performance Report | CTR, VTR, completion |
| Inventory Report | Fill rate, availability |
| Advertiser Report | Per-advertiser metrics |
| Placement Report | Per-placement metrics |

**Custom Report Builder**

| Feature | Options |
|---------|---------|
| Metrics | Select from available |
| Dimensions | Date, Advertiser, Placement, etc. |
| Filters | Apply conditions |
| Date Range | Custom period |
| Granularity | Hour/Day/Week/Month |
| Visualization | Table/Chart/Graph |

**Report Scheduling**

| Setting | Options |
|---------|---------|
| Frequency | Daily/Weekly/Monthly |
| Recipients | Email addresses |
| Format | PDF/Excel/CSV |
| Delivery Time | Scheduled time |

**Export Options**

| Format | Use Case |
|--------|----------|
| PDF | Presentation-ready |
| Excel | Analysis |
| CSV | Data import |
| API | Automated fetch |
| Google Sheets | Live sync |

---

#### 2.9.12 Social Features Moderation

**2.9.12.1 Reviews & Ratings**

**Review Queue**
- Pending reviews for moderation
- Approve / Reject / Edit
- Flag inappropriate content
- Respond to reviews

**Review Filters**

| Filter | Options |
|--------|---------|
| Status | Pending, Approved, Rejected |
| Rating | 1-5 stars |
| Date | Date range |
| Content | Specific title |
| User | Specific user |
| Flagged | Yes / No |

**Moderation Actions**
- Approve review
- Reject with reason
- Edit review (remove profanity)
- Ban user from reviews
- Feature review

**2.9.12.2 Reported Content**

**Report Types**

| Type | Action |
|------|--------|
| Inappropriate Content | Review, remove |
| Copyright Claim | Legal review |
| Technical Issue | Forward to tech |
| Spam | Remove, ban user |

**Report Queue**
- View all reports
- Assign to team member
- Take action
- Close with resolution

**2.9.12.3 Watch Party Moderation**

**Settings**
- Enable/Disable watch parties
- Max participants
- Chat moderation (auto-filter)
- Report handling

---

#### 2.9.13 Analytics & Reporting

**2.9.13.0 Third-Party Analytics Integration (Singular)**

**Singular Integration**

| Feature | Description |
|---------|-------------|
| Provider | Singular (singular.net) |
| Purpose | Mobile attribution, downloads, regional analytics |
| SDK | Singular SDK integrated in mobile apps |
| Dashboard | Singular dashboard access for detailed reports |

**Singular Analytics Capabilities**

| Analytics Type | Metrics |
|----------------|---------|
| Download Analytics | App installs, Install source, Organic vs Paid |
| Attribution | Campaign attribution, Ad network performance |
| Regional Analytics | Installs by country, state, city |
| Device Analytics | Device type, OS version, Manufacturer |
| Retention | D1, D7, D30 retention by source |
| Revenue | In-app revenue attribution |
| Cost Analytics | Ad spend, CPI, ROAS |
| Fraud Prevention | Install validation, Fraud detection |

**Singular Reports (via Integration)**

| Report | Description |
|--------|-------------|
| Install Report | Daily/weekly installs by region, source |
| Attribution Report | Installs attributed to campaigns |
| Cohort Report | User behavior by install cohort |
| ROI Report | Return on ad spend by channel |
| Geographic Report | Performance by country/state/city |
| Creative Report | Performance by ad creative |
| SKAdNetwork | iOS privacy-compliant attribution |

**Admin Access to Singular**

| Access Level | Permissions |
|--------------|-------------|
| View Dashboard | View all Singular reports |
| Export Data | Download reports (CSV, Excel) |
| API Access | Singular API for custom reports |
| Configure Events | Map in-app events to Singular |

**Key Metrics from Singular**

| Metric | Description |
|--------|-------------|
| Total Installs | App downloads across platforms |
| Organic Installs | Non-attributed installs |
| Paid Installs | Ad-attributed installs |
| Install by Region | State/city wise breakdown |
| Install by Source | Play Store, App Store, Direct |
| CPI | Cost per install by campaign |
| Conversion Rate | Install to subscription rate |
| Uninstall Rate | App uninstall tracking |

---

**2.9.13.1 Dashboard Analytics**

**Real-Time Metrics**
- Current active users
- Live streams count
- Current bandwidth usage
- Active regions

**2.9.13.2 User Analytics**

| Report | Metrics |
|--------|---------|
| User Growth | New users, DAU, MAU, Churn |
| Engagement | Session time, Views per user |
| Retention | D1, D7, D30 retention |
| Cohort | Retention by signup cohort |

**2.9.13.3 Content Analytics**

| Report | Metrics |
|--------|---------|
| Content Performance | Views, Watch time, Completion |
| Top Content | Most viewed, Most completed |
| Content Discovery | How users find content |
| Catalog Health | Content by status, age |

**2.9.13.4 Revenue Analytics**

| Report | Metrics |
|--------|---------|
| Revenue Overview | Daily, Weekly, Monthly revenue |
| Revenue by Plan | Breakdown by subscription |
| LTV Analysis | Lifetime value by cohort |
| Payment Analytics | Success rate, Methods used |

**2.9.13.5 Streaming Analytics**

| Report | Metrics |
|--------|---------|
| Bandwidth | Usage by region, time |
| Quality | Bitrate distribution |
| Errors | Playback errors, Buffering |
| CDN Performance | Latency by PoP |

**2.9.13.6 Report Builder**

**Custom Report Creation**
- Select metrics
- Add dimensions
- Set filters
- Choose visualization
- Schedule delivery

**Export Options**
- PDF
- Excel
- CSV
- Google Sheets
- Email scheduled

---

#### 2.9.14 System Configuration

**2.9.14.1 App Configuration**

**General Settings**

| Setting | Description |
|---------|-------------|
| App Name | MahuaPlay |
| Support Email | support@mahuaplay.com |
| Support Phone | +91-XXX-XXX-XXXX |
| Terms URL | Link to terms |
| Privacy URL | Link to privacy policy |

**Feature Flags**

| Flag | Description | Status |
|------|-------------|--------|
| downloads_enabled | Allow downloads | On |
| watch_party_enabled | Watch party feature | On |
| voice_search | Voice search | On |
| pip_mode | Picture-in-picture | On |
| kids_mode | Kids profiles | On |

**2.9.14.2 Regional Settings**

**Language Configuration**

| Language | App UI | Content | Subtitles |
|----------|--------|---------|-----------|
| English | Yes | Yes | Yes |
| Hindi | Yes | Yes | Yes |
| Marathi | Yes | Yes | Yes |
| Gujarati | Yes | Yes | Yes |
| Odia | Yes | Yes | Yes |
| Bhojpuri | No | Yes | Yes |

**Region-Specific Settings**
- Default language by region
- Content availability by region
- Pricing by region
- Payment methods by region

**2.9.14.3 Payment Configuration**

**Payment Gateway Settings**

| Setting | Value |
|---------|-------|
| Gateway | Sabpaisa |
| Mode | Live / Test |
| Merchant ID | XXX |
| API Keys | Encrypted |
| Webhook URL | /api/webhooks/payment |

**Payment Methods**

| Method | Enabled | Min | Max |
|--------|---------|-----|-----|
| UPI | Yes | ₹1 | ₹1,00,000 |
| Credit Card | Yes | ₹1 | ₹1,00,000 |
| Debit Card | Yes | ₹1 | ₹1,00,000 |
| Net Banking | Yes | ₹1 | ₹1,00,000 |
| Wallets | Yes | ₹1 | ₹10,000 |

**2.9.14.4 Email & SMS Templates**

**Email Templates**

| Template | Trigger |
|----------|---------|
| Welcome | User registration |
| Password Reset | Forgot password |
| Payment Success | Successful payment |
| Payment Failed | Failed payment |
| Subscription Expiry | 7 days before expiry |

**SMS Templates**

| Template | Trigger |
|----------|---------|
| OTP | Phone verification |
| Payment Confirm | Payment success |
| Renewal Reminder | Before expiry |

**2.9.14.5 Maintenance Mode**

**Maintenance Settings**
- Enable maintenance mode
- Maintenance message
- Estimated end time
- Allow admin access
- Bypass IPs

---

#### 2.9.15 Admin User Management

**2.9.15.1 Admin Roles**

| Role | Description |
|------|-------------|
| Super Admin | Full access, can manage admins |
| Admin | Full access except admin management |
| Content Manager | Content, CMS, Curation |
| Marketing | Notifications, Campaigns, Promos |
| Finance | Revenue, Refunds, Billing |
| Support | User management, limited actions |
| Analyst | View-only analytics access |
| Moderator | Reviews, Reports, Social |

**2.9.15.2 Permission Matrix**

| Permission | Super Admin | Admin | Content | Marketing | Finance | Support | Analyst | Moderator |
|------------|-------------|-------|---------|-----------|---------|---------|---------|-----------|
| View Dashboard | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| Manage Users | Yes | Yes | No | No | No | Yes | No | No |
| Manage Content | Yes | Yes | Yes | No | No | No | No | No |
| Manage Billing | Yes | Yes | No | No | Yes | No | No | No |
| Send Notifications | Yes | Yes | No | Yes | No | No | No | No |
| View Analytics | Yes | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| System Config | Yes | No | No | No | No | No | No | No |
| Manage Admins | Yes | No | No | No | No | No | No | No |
| Moderate Content | Yes | Yes | Yes | No | No | No | No | Yes |

**2.9.15.3 Admin Actions**

| Action | Description |
|--------|-------------|
| Create Admin | Add new admin user |
| Edit Admin | Modify role, permissions |
| Deactivate Admin | Disable access |
| Delete Admin | Remove admin |
| Reset 2FA | Reset two-factor |
| View Activity | Admin action log |

**2.9.15.4 Admin Activity Log**

| Field | Description |
|-------|-------------|
| Timestamp | Action time |
| Admin | Who performed |
| Action | What was done |
| Target | Affected entity |
| Details | Action details |
| IP Address | Source IP |

---

#### 2.9.16 Audit & Compliance

**2.9.16.1 Audit Logs**

**Logged Events**

| Category | Events |
|----------|--------|
| Authentication | Login, Logout, Failed login |
| User Actions | Create, Edit, Delete, Suspend |
| Content Actions | Upload, Publish, Delete |
| Financial | Refund, Plan change |
| System | Config change, Feature toggle |

**Log Retention**
- Real-time logs: 30 days
- Archived logs: 1 year
- Financial logs: 7 years

**2.9.16.2 GDPR Compliance**

**User Data Management**
- Export user data (JSON/CSV)
- Delete user data (right to erasure)
- Anonymize user data
- Consent management

**Data Export Contents**
- Profile information
- Watch history
- Payment history
- Device history
- Preferences

**2.9.16.3 Security Settings**

| Setting | Configuration |
|---------|---------------|
| Password Policy | Min 8 chars, uppercase, number, special |
| Session Timeout | 30 minutes inactive |
| Max Login Attempts | 5 before lockout |
| Lockout Duration | 30 minutes |
| 2FA Requirement | Mandatory for all admins |
| IP Whitelist | Optional, for enterprise |

**2.9.16.4 Compliance Reports**

| Report | Purpose |
|--------|---------|
| Access Report | Who accessed what |
| Change Report | What was modified |
| Security Report | Login attempts, suspicious activity |
| Data Report | User data access log |

---

#### 2.9.17 Support Tools

**2.9.17.1 User Lookup**

**Quick Search**
- Search by email
- Search by phone
- Search by user ID
- Search by transaction ID

**Diagnostic Info**
- Account status
- Subscription details
- Recent activity
- Device info
- Known issues

**2.9.17.2 Playback Diagnostics**

| Tool | Purpose |
|------|---------|
| Stream Test | Test stream for user |
| DRM Check | Verify DRM status |
| CDN Status | Check CDN delivery |
| Error Lookup | Find playback errors |

**2.9.17.3 Support Tickets**

**Ticket Management**
- View open tickets
- Assign to agent
- Update status
- Internal notes
- User communication

**Ticket Categories**
- Billing / Payment
- Playback Issues
- Account Issues
- Content Requests
- Technical Support
- Feedback

---

#### 2.9.18 API & Integrations

**2.9.18.1 API Keys**

**API Key Management**
- Generate API keys
- Set permissions
- Set rate limits
- Monitor usage
- Revoke keys

**2.9.18.2 Webhooks**

**Webhook Events**

| Event | Payload |
|-------|---------|
| user.created | User details |
| subscription.created | Subscription details |
| payment.success | Payment details |
| content.published | Content details |

**Webhook Configuration**
- Endpoint URL
- Secret key
- Events to send
- Retry policy

**2.9.18.3 Third-Party Integrations**

| Integration | Purpose | Status |
|-------------|---------|--------|
| Sabpaisa | Payments | Active |
| Firebase | Push notifications | Active |
| AWS SES | Email | Active |
| MSG91 | SMS | Active |
| Mixpanel | Analytics | Active |
| Sentry | Error tracking | Active |

---

## 3. Technical Architecture

### 3.1 High-Level Architecture

**Client Layer**

| Platform | Technology |
|----------|------------|
| Web | React.js |
| iOS | Swift |
| Android | Kotlin + Jetpack Compose |
| Smart TV | Tizen/WebOS |
| Fire TV/Roku | Native apps |

**Infrastructure Flow**
```
Clients --> CloudFront/Bunny CDN --> API Gateway --> ECS/Lambda --> Aurora/DynamoDB
                    |
                    v
              Bunny Stream --> Video Delivery
```

### 3.2 Microservices Architecture

**Core Services**

| Service | Responsibility | Technology |
|---------|----------------|------------|
| Auth Service | User authentication, sessions | Node.js + Cognito |
| User Service | Profiles, preferences | Node.js |
| Content Service | Metadata, catalog | Node.js |
| Playback Service | Stream URLs, DRM tokens | Node.js |
| Search Service | Search, recommendations | Node.js + OpenSearch |
| Payment Service | Subscriptions, billing | Node.js |
| Notification Service | Push, email, SMS | Node.js + SQS |
| Analytics Service | Event tracking | Node.js + Kinesis |

### 3.3 Apache Kafka Event Streaming Architecture

**Kafka Cluster (AWS MSK)**
- 3 brokers (production)
- Replication factor: 3
- Retention: 7 days

**Key Topics**

| Topic | Purpose | Partitions | Consumers |
|-------|---------|------------|-----------|
| user-events | Login, signup, profile changes | 12 | Analytics, Notification, Singular |
| playback-events | Play, pause, seek, complete | 24 | Analytics, Recommendation, Billing |
| content-events | Upload, publish, update | 6 | Search Index, CDN Invalidation |
| payment-events | Subscribe, renew, cancel | 6 | Notification, Analytics, Sabpaisa Webhook |
| notification-events | Push, email, SMS, WhatsApp triggers | 12 | FCM, SES, MSG91, WhatsApp API |
| ad-events | Impressions, clicks, completions | 12 | Ad Analytics, Revenue Dashboard |
| admin-events | Admin actions, config changes | 6 | Audit Log, Analytics |
| alert-events | System alerts, errors | 6 | Admin Dashboard, Sentry |

**Event Flow Architecture**
```
User/App Events --> Kafka --> Event Consumers
                              ├── Analytics Service --> Mixpanel/Singular
                              ├── Notification Service --> FCM/SES/MSG91/WhatsApp
                              ├── Recommendation Engine --> ML Pipeline
                              ├── Billing Service --> Sabpaisa
                              ├── Search Indexer --> OpenSearch
                              ├── Admin Dashboard --> Real-time Widgets
                              └── Audit Logger --> Aurora PostgreSQL
```

**Event Consumers**
- Analytics pipeline (real-time dashboards, Mixpanel, Singular)
- Recommendation engine (ML training, personalization)
- Notification service (FCM, SES, MSG91, WhatsApp Business API)
- Billing service (usage tracking, Sabpaisa reconciliation)
- Admin Dashboard (real-time metrics via WebSocket)
- Audit service (compliance logging)

### 3.4 CDN & Video Delivery Infrastructure (India-Focused)

**Primary CDN: Bunny.net**
- 14+ PoPs in India
- Mumbai, Delhi, Bangalore, Chennai, Hyderabad, Kolkata, Pune, Ahmedabad
- Integrated DRM (Widevine, FairPlay)
- Built-in video player

**Backup CDN: CloudFront**
- Edge locations across India
- Origin shield enabled
- Custom SSL

**Delivery Strategy**

| Content Type | CDN | Cache TTL |
|--------------|-----|-----------|
| Video segments | Bunny | 24 hours |
| Images | CloudFront | 7 days |
| API responses | CloudFront | 5 minutes |
| Static assets | CloudFront | 30 days |

### 3.5 Database Schema (Core Tables)

**Users Table**
- user_id (PK)
- email
- phone
- password_hash
- subscription_status
- created_at

**Profiles Table**
- profile_id (PK)
- user_id (FK)
- name
- avatar
- preferences (JSON)
- is_kids

**Content Table**
- content_id (PK)
- title
- description
- type (movie/series)
- release_year
- duration
- rating
- bunny_video_id

**Watch History Table**
- history_id (PK)
- profile_id (FK)
- content_id (FK)
- position_seconds
- completed
- watched_at

**Subscriptions Table**
- subscription_id (PK)
- user_id (FK)
- plan_id
- status
- start_date
- end_date
- payment_method

### 3.6 Third-Party Service Integration (Hybrid Architecture)

#### 3.6.1 Bunny.net Integration

**Services Used**

| Service | Purpose | Admin Portal Section |
|---------|---------|---------------------|
| Bunny Stream | Video hosting, transcoding | 2.9.4 Content Management |
| Bunny CDN | Global video delivery, 14+ India PoPs | 2.9.7 Video Player Config |
| Bunny Player | Embedded DRM player | 2.9.7 Video Player Config |
| Bunny Storage | Raw video storage | 2.9.4 Content Management |
| Bunny DRM | Widevine + FairPlay protection | 2.9.7.8 DRM & Security |

**API Integration**

| Endpoint | Purpose | Used By |
|----------|---------|---------|
| POST /library/{id}/videos | Upload video | Admin CMS, Content Upload |
| GET /library/{id}/videos/{videoId} | Get video details | Content Service |
| DELETE /library/{id}/videos/{videoId} | Delete video | Admin CMS |
| GET /library/{id}/videos/{videoId}/play | Get playback URL | Playback Service |
| POST /library/{id}/videos/{videoId}/captions | Add subtitles | Admin CMS |
| GET /library/{id}/statistics | Video analytics | Admin Dashboard |

**Transcoding Profiles (Auto-generated by Bunny)**

| Quality | Resolution | Bitrate | Plan Access |
|---------|------------|---------|-------------|
| 240p | 426x240 | 300 Kbps | All (Data Saver) |
| 360p | 640x360 | 600 Kbps | All |
| 480p | 854x480 | 1 Mbps | All (Free max) |
| 720p | 1280x720 | 2.5 Mbps | Basic+ |
| 1080p | 1920x1080 | 5 Mbps | Standard+ |
| 4K | 3840x2160 | 15 Mbps | Premium only |

**DRM Configuration**

| Platform | DRM System | Bunny Support |
|----------|------------|---------------|
| Android | Widevine L1/L3 | Built-in |
| iOS/Safari | FairPlay | Built-in |
| Chrome/Firefox | Widevine | Built-in |
| Edge | PlayReady | Built-in |

#### 3.6.2 AWS Services

| Service | Purpose |
|---------|---------|
| Cognito | User authentication |
| Aurora | Primary database |
| DynamoDB | Session storage |
| ElastiCache | Redis caching |
| MSK | Kafka streaming |
| OpenSearch | Search engine |
| SQS | Message queues |
| Lambda | Serverless functions |
| CloudWatch + Grafana | Monitoring |

**Third-Party Integrations**

| Category | Provider | Purpose | Integration Type |
|----------|----------|---------|------------------|
| Payment Gateway | Sabpaisa | UPI, Cards, Net Banking, Wallets | Redirect-based |
| Push Notifications | Firebase Cloud Messaging | Android + iOS push | SDK + Server |
| Email | AWS SES | Transactional, Marketing emails | API |
| SMS | MSG91 | OTP, Notifications | API |
| WhatsApp | WhatsApp Business API | Promotional, Reminders | API |
| Mobile Analytics | Singular | Attribution, Downloads, Regional | SDK |
| User Analytics | Mixpanel | User behavior tracking | SDK + API |
| Crash Reporting | Sentry | Error monitoring | SDK |
| Ad Server | Google Ad Manager | AVOD monetization, VAST/VPAID | SDK + API |
| Ad Networks | Meta, InMobi, Amazon | Programmatic ads | SDK |

**Development Stack**

| Component | Technology |
|-----------|------------|
| Android App | Kotlin + Jetpack Compose |
| Video Player | Bunny Player (WebView) + ExoPlayer (offline) |
| API Backend | Node.js + Express |
| Admin CMS | React.js + Next.js |
| Infrastructure | Terraform + AWS CDK |

#### 3.6.3 Android App Integration

**Dependencies**
- Core: androidx.core, lifecycle-runtime-ktx
- UI: Jetpack Compose, Material3
- Video: ExoPlayer (media3)
- Networking: Retrofit, OkHttp
- DI: Hilt
- Firebase: Cloud Messaging
- Chrome Custom Tabs (for Sabpaisa redirect)

**Video Player Implementation**
- Primary: Bunny Player (WebView embed with DRM)
- Fallback: ExoPlayer for offline/downloaded content

#### 3.6.4 Cost Comparison

| Component | Self-Hosted (AWS) | Hybrid (Bunny + AWS) | Savings |
|-----------|-------------------|----------------------|---------|
| Video Storage (10TB) | $230/mo | $100/mo | 57% |
| Transcoding (1000 hrs) | $900/mo | $60/mo | 93% |
| CDN (50TB/mo) | $4,250/mo | $500/mo | 88% |
| DRM License Server | $500/mo | $0 (Included) | 100% |
| Player Development | $5,000 (one-time) | $0 (Included) | 100% |
| Backend Services | $2,000/mo | $2,000/mo | 0% |
| **Total Monthly** | ~$7,880/mo | ~$2,660/mo | **66%** |

#### 3.6.5 Service Responsibility Matrix

| Responsibility | Bunny.net | AWS | In-House |
|----------------|-----------|-----|----------|
| Video Upload | Yes | | |
| Transcoding | Yes | | |
| CDN Delivery | Yes | | |
| DRM | Yes | | |
| Video Player | Yes | | |
| User Authentication | | Cognito | |
| User Database | | Aurora | |
| Content Metadata | | Aurora | |
| Search | | OpenSearch | |
| Caching | | ElastiCache | |
| Event Streaming | | MSK | |
| Payment Processing | | | Sabpaisa (Redirect) |
| Push Notifications | | | Firebase |
| Android App | | | Kotlin |
| Admin CMS | | | React |
| API Development | | | Node.js |

#### 3.6.6 API Endpoints

**Authentication (Cognito)**
- POST /api/v1/auth/register
- POST /api/v1/auth/login
- POST /api/v1/auth/refresh
- POST /api/v1/auth/logout

**User & Profiles**
- GET /api/v1/users/me
- PUT /api/v1/users/me
- GET /api/v1/profiles
- POST /api/v1/profiles
- PUT /api/v1/profiles/:id

**Content**
- GET /api/v1/content
- GET /api/v1/content/:id
- GET /api/v1/content/:id/playback (Returns Bunny stream URL)
- GET /api/v1/content/featured
- GET /api/v1/content/trending

**Search**
- GET /api/v1/search?q=
- GET /api/v1/search/suggestions

**Subscriptions & Payments**
- GET /api/v1/subscriptions/plans
- POST /api/v1/subscriptions/checkout (Returns Sabpaisa redirect URL)
- POST /api/v1/subscriptions/verify
- GET /api/v1/subscriptions/status

**Watchlist & History**
- GET /api/v1/watchlist
- POST /api/v1/watchlist/:contentId
- DELETE /api/v1/watchlist/:contentId
- GET /api/v1/history
- POST /api/v1/history/progress

**Admin CMS (Internal)**
- POST /api/v1/admin/content (Triggers Bunny upload)
- PUT /api/v1/admin/content/:id
- DELETE /api/v1/admin/content/:id
- GET /api/v1/admin/analytics

---

## 4. Non-Functional Requirements

### 4.1 Performance

| Metric | Target | Measurement |
|--------|--------|-------------|
| Video Start Time | < 2 seconds | P95 |
| Seek Latency | < 500ms | P95 |
| API Response Time | < 200ms | P95 |
| Page Load Time | < 3 seconds | P95 |
| Search Results | < 500ms | P95 |

### 4.2 Scalability

| Metric | Target |
|--------|--------|
| Concurrent Streams | 10 million+ |
| Registered Users | 100 million+ |
| Content Library | 50,000+ titles |
| Daily Active Users | 20 million+ |
| Peak Traffic | 5x average |

### 4.3 Availability

| Component | Target SLA |
|-----------|------------|
| Streaming Service | 99.99% |
| API Services | 99.95% |
| Admin Portal | 99.9% |

### 4.4 Security

| Requirement | Implementation |
|-------------|----------------|
| Data Encryption | TLS 1.3 in transit, AES-256 at rest |
| DRM | Widevine L1, FairPlay, PlayReady |
| Authentication | JWT + Refresh tokens |
| Payment Security | PCI-DSS compliant |
| Content Protection | Watermarking, Screenshot prevention |

---

## 5. Platform Support

### 5.1 Supported Devices

| Platform | Min Version | Priority |
|----------|-------------|----------|
| Web (Chrome, Firefox, Safari, Edge) | Latest 2 versions | P0 |
| iOS | iOS 14+ | P0 |
| Android | Android 8+ | P0 |
| Android TV | Android TV 9+ | P1 |
| Apple TV | tvOS 14+ | P1 |
| Fire TV | Fire OS 6+ | P1 |
| Roku | Roku OS 10+ | P2 |
| Samsung Smart TV | Tizen 5+ | P2 |
| LG Smart TV | WebOS 5+ | P2 |
| PlayStation | PS4/PS5 | P3 |
| Xbox | Xbox One/Series | P3 |

### 5.2 Supported Regions (Phase 1)

| Region | Languages | Payment Methods |
|--------|-----------|-----------------|
| India | Hindi, English, Tamil, Telugu | UPI, Cards, Wallets |
| USA | English, Spanish | Cards, PayPal |
| UK | English | Cards, PayPal |
| Canada | English, French | Cards |

---

## 6. Analytics & Metrics

### 6.1 Key Performance Indicators (KPIs)

| Category | Metric | Target |
|----------|--------|--------|
| **Acquisition** | New Signups/Month | 500K+ |
| **Engagement** | Daily Active Users | 20M+ |
| **Engagement** | Avg. Watch Time/User | 2+ hours |
| **Retention** | Monthly Churn Rate | <5% |
| **Revenue** | ARPU (Avg Revenue Per User) | $10+ |
| **Content** | Content Completion Rate | 70%+ |
| **Technical** | Buffering Ratio | <0.5% |

### 6.2 Tracking Events

```javascript
// User Events
user_signup, user_login, profile_switch, subscription_change

// Content Events
content_browse, content_search, content_click, content_play,
content_pause, content_seek, content_complete, content_rate

// Engagement Events
watchlist_add, watchlist_remove, share_content, download_start,
download_complete, notification_click

// Technical Events
video_start, video_buffer, video_error, quality_change,
latency_measure, crash_report
```

---

## 7. Monetization

### 7.1 Revenue Streams

| Stream | Description | Target Revenue % |
|--------|-------------|------------------|
| Subscriptions | Monthly/Yearly plans | 60% |
| Advertising | AVOD + Hybrid tiers | 25% |
| Premium Content | Pay-per-view, Early access | 10% |
| Merchandise | Branded merchandise | 5% |

### 7.2 Ad Revenue Model

#### Revenue Tiers by Ad Type
| Ad Type | eCPM Range | Revenue/1M Views |
|---------|------------|------------------|
| Pre-Roll (15s) | $15-25 | $15,000-25,000 |
| Pre-Roll (30s) | $20-35 | $20,000-35,000 |
| Mid-Roll | $25-45 | $25,000-45,000 |
| Pause Ads | $5-10 | $5,000-10,000 |
| Banner Overlay | $3-8 | $3,000-8,000 |

#### Ad Revenue Projections
```
Monthly Active Users (AVOD): 5 Million
Average Watch Time: 2 hours/day
Ads per Hour: 4 (for free tier)

Daily Ad Impressions: 5M Ã— 2hr Ã— 4ads = 40M impressions
Monthly Ad Impressions: 40M Ã— 30 = 1.2B impressions

At $20 eCPM (average):
Monthly Ad Revenue = (1.2B / 1000) Ã— $20 = $24 Million
```

#### Advertiser Pricing
| Package | Impressions | Price | CPM |
|---------|-------------|-------|-----|
| Starter | 100K | $2,500 | $25 |
| Growth | 500K | $10,000 | $20 |
| Premium | 1M | $18,000 | $18 |
| Enterprise | 5M+ | Custom | $12-15 |

### 7.3 Pricing Strategy

```
Free Tier (Ad-supported):
â”œâ”€â”€ Limited content library
â”œâ”€â”€ 720p max quality
â”œâ”€â”€ 2-3 ads per hour
â””â”€â”€ No downloads

Paid Tiers:
â”œâ”€â”€ Basic: $7.99/mo - 1 screen, 720p, Mobile only
â”œâ”€â”€ Standard: $12.99/mo - 2 screens, 1080p, All devices
â”œâ”€â”€ Premium: $19.99/mo - 4 screens, 4K HDR, Downloads
â””â”€â”€ Annual: 2 months free on yearly subscription
```

---

## 8. Roadmap

### Phase 1: MVP (Months 1-4)
- [ ] User authentication (Email, Google)
- [ ] Basic video player with adaptive streaming
- [ ] Content catalog with search
- [ ] Single profile per account
- [ ] Basic subscription (1 tier)
- [ ] Web + Mobile apps

### Phase 2: Growth (Months 5-8)
- [ ] Multiple profiles
- [ ] Recommendation engine v1
- [ ] Downloads for offline viewing
- [ ] Multiple subscription tiers
- [ ] Smart TV apps
- [ ] Kids profile with parental controls

### Phase 3: Engagement (Months 9-12)
- [ ] Watch Party feature
- [ ] Social features (ratings, reviews)
- [ ] Advanced recommendations (ML)
- [ ] Live streaming capability
- [ ] Original content production tools

### Phase 4: Scale (Year 2)
- [ ] International expansion
- [ ] Ad-supported free tier
- [ ] Gaming integration
- [ ] Interactive content
- [ ] VR/AR experiences

---

## 9. Success Criteria

### 9.1 Launch Criteria (MVP)

| Criteria | Target |
|----------|--------|
| Core Features Complete | 100% of P0 features |
| Bug-Free Release | 0 critical, <5 major bugs |
| Performance | All P95 targets met |
| Security Audit | Pass with no critical issues |
| Load Testing | Handle 100K concurrent users |

### 9.2 Success Metrics (6 Months Post-Launch)

| Metric | Target |
|--------|--------|
| Registered Users | 1 million+ |
| Paid Subscribers | 200K+ |
| Daily Active Users | 100K+ |
| App Store Rating | 4.0+ stars |
| NPS Score | 40+ |

---

## 10. Risks & Mitigations

| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Content licensing delays | High | Medium | Start negotiations early, have backup content |
| High infrastructure costs | High | High | Use reserved instances, optimize CDN usage |
| DRM implementation issues | High | Medium | Use established DRM providers (BuyDRM) |
| Piracy/Content leaks | High | Medium | Implement watermarking, monitor torrent sites |
| Poor video quality | High | Low | Extensive QA, multiple CDN fallbacks |
| Competitor launches | Medium | High | Focus on unique features, regional content |

---

## 11. Appendix

### 11.1 Glossary

| Term | Definition |
|------|------------|
| OTT | Over-The-Top - content delivered via internet |
| CDN | Content Delivery Network |
| DRM | Digital Rights Management |
| HLS | HTTP Live Streaming (Apple protocol) |
| DASH | Dynamic Adaptive Streaming over HTTP |
| ABR | Adaptive Bitrate Streaming |
| ARPU | Average Revenue Per User |
| MAU | Monthly Active Users |
| DAU | Daily Active Users |

### 11.2 References

- Netflix Tech Blog: https://netflixtechblog.com
- AWS Media Services: https://aws.amazon.com/media-services/
- Video.js Documentation: https://videojs.com
- HLS Specification: https://developer.apple.com/streaming/

---

## 12. Tech Stack Summary (For Development Team)

### Frontend
| Platform | Technology | Notes |
|----------|------------|-------|
| Web | React.js + Next.js | SSR for SEO, PWA support |
| Android | Kotlin + Jetpack Compose | ExoPlayer for video |
| iOS | Swift + SwiftUI | AVPlayer for video |
| Android TV | Kotlin + Leanback | TV-optimized UI |
| Fire TV | Kotlin | Amazon ecosystem |

### Backend
| Service | Technology | Database |
|---------|------------|----------|
| User Service | Node.js + Express | PostgreSQL |
| Content Service | Python + FastAPI | MongoDB + Elasticsearch |
| Streaming Service | Go | Redis |
| Payment Service | Node.js | PostgreSQL |
| Recommendation | Python + TensorFlow | Redis + S3 |
| Notification | Node.js | Redis + SQS |
| Analytics | Python + Spark | ClickHouse |

### Infrastructure (Hybrid Architecture)

#### Video Pipeline (Bunny.net)
| Component | Service | Notes |
|-----------|---------|-------|
| Video Storage | Bunny Storage | Raw upload storage |
| Transcoding | Bunny Stream | Auto ABR encoding (240p-4K) |
| CDN Delivery | Bunny CDN | India PoPs (Mumbai, Chennai, Delhi) |
| DRM | Bunny DRM Add-on | Widevine + FairPlay (built-in) |
| Video Player | Bunny Player | Embeddable, no development needed |

#### Backend (AWS Mumbai)
| Component | Service | Notes |
|-----------|---------|-------|
| Compute | ECS Fargate | Containerized API services |
| Database | Aurora PostgreSQL | Serverless v2, auto-scaling |
| Cache | ElastiCache Redis | 6-node cluster |
| Search | OpenSearch | Content search, multilingual |
| Auth | Cognito | User authentication |
| Events | Amazon MSK | Kafka for event streaming |
| Storage | S3 | Thumbnails, assets (NOT videos) |
| Functions | Lambda | Webhooks, async processing |
| Monitoring | CloudWatch + Grafana | Metrics and alerting |

### Third-Party Integrations

| Category | Provider | Purpose | Admin Config Section |
|----------|----------|---------|---------------------|
| Video CDN | Bunny.net | Video hosting, transcoding, CDN, DRM | 2.9.7 Video Player |
| Payment | Sabpaisa | UPI, Cards, Net Banking, Wallets | 2.9.14.3 Payment Config |
| Push | Firebase Cloud Messaging | Android + iOS push notifications | 2.9.10 Notifications |
| Email | AWS SES | Transactional, Marketing emails | 2.9.10 Notifications |
| SMS | MSG91 | OTP, Notifications | 2.9.10 Notifications |
| WhatsApp | WhatsApp Business API | Promotional, Reminders | 2.9.10 Notifications |
| Mobile Attribution | Singular | Downloads, Attribution, Regional | 2.9.13.0 Analytics |
| User Analytics | Mixpanel | User behavior, Events | 2.9.13 Analytics |
| Crash Reporting | Sentry | Error monitoring, Crash reports | 2.9.17.2 Playback Diagnostics |
| Ad Server | Google Ad Manager | VAST/VPAID, Programmatic | 2.9.11 Advertisement |
| Ad Networks | Meta, InMobi, Amazon | Mobile/TV ads | 2.9.11.1 Ad Networks |
| Search | AWS OpenSearch | Content search, Recommendations | 2.9.8 Search Config |
| Event Streaming | AWS MSK (Kafka) | Real-time events, Analytics | All real-time features |

### Development Stack
| Component | Technology |
|-----------|------------|
| Android App | Kotlin + Jetpack Compose |
| Video Player | Bunny Player (WebView) + ExoPlayer (offline) |
| API Backend | Node.js + Express |
| Admin CMS | React.js + Next.js |
| Infrastructure | Terraform + AWS CDK |

---

## 13. Integration Matrix & Developer Reference

### 13.1 Feature-to-Service Mapping

This matrix shows which third-party services power each feature. Use this as a reference when implementing features.

#### User-Facing Features

| Feature | Primary Services | Kafka Topics | Admin Config |
|---------|------------------|--------------|--------------|
| User Registration | AWS Cognito, MSG91 (OTP) | user-events | 2.9.3 User Mgmt |
| Login (Google) | AWS Cognito, Google OAuth | user-events | 2.9.3 User Mgmt |
| Login (Phone OTP) | AWS Cognito, MSG91 | user-events | 2.9.14.4 SMS Templates |
| Profile Management | Aurora PostgreSQL | user-events | 2.9.3.2 User Profile |
| Video Playback | Bunny Stream, Bunny CDN, Bunny DRM | playback-events | 2.9.7 Video Player |
| Search | AWS OpenSearch | - | 2.9.8 Search Config |
| Recommendations | OpenSearch, ML Pipeline | playback-events | 2.9.9 Recommendations |
| Subscription Purchase | Sabpaisa | payment-events | 2.9.5 Subscription Mgmt |
| Push Notifications | Firebase Cloud Messaging | notification-events | 2.9.10 Notifications |
| Email Notifications | AWS SES | notification-events | 2.9.10.5 Templates |
| SMS Notifications | MSG91 | notification-events | 2.9.14.4 SMS Templates |
| WhatsApp Messages | WhatsApp Business API | notification-events | 2.9.10.1 Channels |
| Ads (Free Tier) | Google Ad Manager, Meta, InMobi | ad-events | 2.9.11 Advertisement |
| Downloads | Bunny CDN, ExoPlayer | playback-events | 2.9.7.5 Playback Rules |
| Watch Party | WebSocket Service | playback-events | 2.9.12.3 Watch Party |

#### Admin Features

| Feature | Primary Services | Kafka Topics | Data Source |
|---------|------------------|--------------|-------------|
| Real-time Dashboard | WebSocket, Redis | All events | 2.9.2 Dashboard |
| Content Upload | Bunny Stream API | content-events | 2.9.4 Content Mgmt |
| Transcoding Status | Bunny Stream Webhook | content-events | 2.9.4 Content Mgmt |
| User Analytics | Mixpanel, Singular | user-events, playback-events | 2.9.13 Analytics |
| Revenue Analytics | Aurora, Sabpaisa | payment-events | 2.9.5.3 Revenue |
| Ad Analytics | Google Ad Manager | ad-events | 2.9.11.11 Ad Analytics |
| Notification Campaigns | FCM, SES, MSG91, WhatsApp | notification-events | 2.9.10.7 Campaigns |
| Audit Logs | Aurora PostgreSQL | admin-events | 2.9.16.1 Audit Logs |

---

### 13.2 API Integration Quick Reference

#### Bunny.net APIs (Video Pipeline)

```
Base URL: https://video.bunnycdn.com

# Upload Video
POST /library/{libraryId}/videos
Headers: AccessKey: {API_KEY}
Body: { "title": "Movie Name" }
Response: { "guid": "video-id", "status": "created" }

# Get Playback URL (with DRM)
GET /library/{libraryId}/videos/{videoId}/play
Response: { "playbackUrl": "https://...", "drmToken": "..." }

# Webhook Events (configure in Bunny dashboard)
- video.encoded (transcoding complete)
- video.deleted
- video.captions.added
```

#### Sabpaisa Payment (Redirect Flow)

```
# Step 1: Create Order (Backend)
POST /api/v1/subscriptions/checkout
Body: { "planId": "premium_monthly", "userId": "..." }
Response: { "redirectUrl": "https://sabpaisa.com/pay/...", "orderId": "..." }

# Step 2: User completes payment on Sabpaisa page

# Step 3: Webhook Callback (Sabpaisa → Backend)
POST /api/webhooks/sabpaisa
Body: { "orderId": "...", "status": "success", "transactionId": "..." }
Action: Update subscription, publish to payment-events Kafka topic
```

#### Firebase Cloud Messaging (Push)

```
# Send Notification (Backend)
POST https://fcm.googleapis.com/fcm/send
Headers: Authorization: key={SERVER_KEY}
Body: {
  "to": "{device_token}",
  "notification": { "title": "...", "body": "..." },
  "data": { "contentId": "...", "deepLink": "..." }
}

# Triggered by: notification-events Kafka consumer
```

#### Singular SDK (Mobile Attribution)

```kotlin
// Android Integration
Singular.init(context, config)

// Track Install (automatic)
// Track Events
Singular.event("subscription_purchase", mapOf("plan" to "premium", "revenue" to 499))
Singular.event("video_play", mapOf("contentId" to "..."))

// Admin Dashboard: View data at singular.net dashboard
```

#### AWS OpenSearch (Search)

```
# Index Content
PUT /content/_doc/{contentId}
Body: { "title": "...", "genres": [...], "cast": [...], "language": "..." }

# Search
GET /content/_search
Body: {
  "query": {
    "multi_match": {
      "query": "action movie",
      "fields": ["title^10", "cast^6", "description^3"]
    }
  }
}
```

---

### 13.3 Kafka Event Schemas

All events follow this base structure:

```json
{
  "eventId": "uuid",
  "eventType": "user.login",
  "timestamp": "2026-02-19T10:30:00Z",
  "source": "android-app",
  "userId": "uuid",
  "data": { ... }
}
```

#### Key Event Types

| Topic | Event Type | Data Fields | Consumers |
|-------|------------|-------------|-----------|
| user-events | user.signup | email, phone, method, referralCode | Analytics, Notification |
| user-events | user.login | deviceId, ip, location | Analytics, Security |
| playback-events | playback.start | contentId, quality, deviceType | Analytics, Recommendations |
| playback-events | playback.progress | contentId, position, duration | Continue Watching |
| playback-events | playback.complete | contentId, watchTime | Recommendations, Analytics |
| payment-events | payment.success | planId, amount, method | Notification, Analytics |
| payment-events | subscription.expired | planId, userId | Notification, Ad Service |
| content-events | content.published | contentId, title, type | Search Index, CDN Warm |
| notification-events | notification.send | channel, template, userId | FCM/SES/MSG91/WhatsApp |
| ad-events | ad.impression | adId, placement, revenue | Ad Analytics |
| admin-events | admin.action | adminId, action, target | Audit Log |

---

### 13.4 Environment Configuration

#### Required API Keys & Secrets

| Service | Secret Name | Where Used |
|---------|-------------|------------|
| Bunny.net | BUNNY_API_KEY | Content Service, Admin CMS |
| Bunny.net | BUNNY_LIBRARY_ID | Content Service |
| AWS Cognito | COGNITO_USER_POOL_ID | Auth Service |
| AWS Cognito | COGNITO_CLIENT_ID | Auth Service, Apps |
| Sabpaisa | SABPAISA_MERCHANT_ID | Payment Service |
| Sabpaisa | SABPAISA_SECRET_KEY | Payment Service |
| Firebase | FCM_SERVER_KEY | Notification Service |
| MSG91 | MSG91_AUTH_KEY | Notification Service |
| MSG91 | MSG91_SENDER_ID | Notification Service |
| WhatsApp | WHATSAPP_BUSINESS_ID | Notification Service |
| WhatsApp | WHATSAPP_ACCESS_TOKEN | Notification Service |
| Singular | SINGULAR_API_KEY | Android/iOS Apps |
| Singular | SINGULAR_SECRET | Analytics Service |
| Mixpanel | MIXPANEL_TOKEN | Analytics Service, Apps |
| Sentry | SENTRY_DSN | All Services, Apps |
| Google Ad Manager | GAM_NETWORK_ID | Ad Service |
| AWS MSK | KAFKA_BOOTSTRAP_SERVERS | All Services |
| OpenSearch | OPENSEARCH_ENDPOINT | Search Service |

#### Infrastructure Endpoints

| Environment | API Gateway | Admin Portal | CDN |
|-------------|-------------|--------------|-----|
| Development | api-dev.mahuaplay.com | admin-dev.mahuaplay.com | dev-cdn.b-cdn.net |
| Staging | api-staging.mahuaplay.com | admin-staging.mahuaplay.com | staging-cdn.b-cdn.net |
| Production | api.mahuaplay.com | admin.mahuaplay.com | cdn.mahuaplay.com |

---

### 13.5 Development Workflow

#### Content Upload Flow
```
1. Admin uploads video via Admin CMS (2.9.4)
2. Backend calls Bunny Stream API → Returns video GUID
3. Publish content-events/content.uploaded to Kafka
4. Bunny transcodes video (240p-4K automatically)
5. Bunny sends webhook → content-events/content.encoded
6. Backend updates content status to "ready"
7. Content appears in app via Content Service API
```

#### Subscription Flow
```
1. User selects plan in app
2. App calls POST /api/v1/subscriptions/checkout
3. Backend creates order, returns Sabpaisa redirect URL
4. User completes payment on Sabpaisa page
5. Sabpaisa sends webhook to /api/webhooks/sabpaisa
6. Backend publishes payment-events/payment.success to Kafka
7. Notification Service sends confirmation (FCM, Email, SMS)
8. User's subscription is activated
```

#### Notification Flow
```
1. Event occurs (new content, payment, etc.)
2. Service publishes to notification-events Kafka topic
3. Notification Service consumes event
4. Checks user preferences (Admin 2.9.10.8)
5. Routes to appropriate channel:
   - Push → Firebase Cloud Messaging
   - Email → AWS SES
   - SMS → MSG91
   - WhatsApp → WhatsApp Business API
6. Logs delivery status to Analytics
```

---

### 13.6 Quick Links for Developers

| Resource | URL |
|----------|-----|
| Bunny.net API Docs | https://docs.bunny.net/reference/api-overview |
| Bunny Stream Docs | https://docs.bunny.net/docs/stream-api |
| AWS Cognito Docs | https://docs.aws.amazon.com/cognito/ |
| Sabpaisa Integration | https://docs.sabpaisa.com/ |
| Firebase FCM | https://firebase.google.com/docs/cloud-messaging |
| MSG91 API | https://docs.msg91.com/ |
| WhatsApp Business API | https://developers.facebook.com/docs/whatsapp |
| Singular SDK | https://support.singular.net/hc/en-us/categories/360002441212-Developer-Resources |
| Mixpanel SDK | https://developer.mixpanel.com/ |
| AWS MSK (Kafka) | https://docs.aws.amazon.com/msk/ |
| OpenSearch | https://opensearch.org/docs/latest/ |

---

**End of Document**

*This PRD is ready for development. All third-party integrations are documented with their corresponding Admin Portal configuration sections. For questions, contact the Product Team.*
