# Product Requirements Document (PRD)
# OTT Streaming Platform - "MahuaPlay"

**Document Version**: 2.0
**Last Updated**: 17 February 2026
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

| Topic | Purpose | Partitions |
|-------|---------|------------|
| user-events | Login, signup, profile changes | 12 |
| playback-events | Play, pause, seek, complete | 24 |
| content-events | Upload, publish, update | 6 |
| payment-events | Subscribe, renew, cancel | 6 |
| notification-events | Push, email triggers | 12 |

**Event Consumers**
- Analytics pipeline (real-time dashboards)
- Recommendation engine (ML training)
- Notification service (trigger alerts)
- Billing service (usage tracking)

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
- Bunny Stream (video hosting, transcoding)
- Bunny CDN (global delivery)
- Bunny Player (embedded player with DRM)

**API Integration**

| Endpoint | Purpose |
|----------|---------|
| POST /library/{id}/videos | Upload video |
| GET /library/{id}/videos/{videoId} | Get video details |
| DELETE /library/{id}/videos/{videoId} | Delete video |
| GET /library/{id}/videos/{videoId}/play | Get playback URL |

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

### Third-Party Integrations

| Category | Provider | Notes |
|----------|----------|-------|
| Payment Gateway | Sabpaisa | Redirect-based (UPI, Cards, Wallets) |
| Push Notifications | Firebase Cloud Messaging | Android + iOS |
| Email | AWS SES | Transactional emails |
| SMS | MSG91 | OTP, notifications |
| Analytics | Mixpanel | User behavior tracking |
| Crash Reporting | Sentry | Error monitoring |
| Ad Server | Google Ad Manager | AVOD monetization |

### Development Stack

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
| Category | Provider | Notes |
|----------|----------|-------|
| Payment Gateway | Sabpaisa | Redirect-based (UPI, Cards, Wallets) |
| Push Notifications | Firebase Cloud Messaging | Android + iOS |
| Email | AWS SES | Transactional emails |
| SMS | MSG91 | OTP, notifications |
| Analytics | Mixpanel | User behavior tracking |
| Crash Reporting | Sentry | Error monitoring |
| Ad Server | Google Ad Manager | AVOD monetization |

### Development Stack
| Component | Technology |
|-----------|------------|
| Android App | Kotlin + Jetpack Compose |
| Video Player | Bunny Player (WebView) + ExoPlayer (offline) |
| API Backend | Node.js + Express |
| Admin CMS | React.js + Next.js |
| Infrastructure | Terraform + AWS CDK |

---

**End of Document**

*This PRD is ready for development. For questions, contact the Product Team.*
