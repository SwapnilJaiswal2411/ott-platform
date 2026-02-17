# Product Requirements Document (PRD)
# OTT Streaming Platform - "StreamVerse"

---

## 1. Executive Summary

### 1.1 Product Vision
StreamVerse is a next-generation Over-The-Top (OTT) streaming platform that delivers high-quality video content to users across multiple devices. Similar to Netflix, it provides on-demand movies, TV shows, documentaries, and original content with a personalized viewing experience.

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

#### 2.1.1 Authentication & Authorization
| Feature | Description | Priority |
|---------|-------------|----------|
| Email/Password Login | Standard authentication | P0 |
| Social Login | Google, Facebook, Apple | P0 |
| Phone OTP Login | Mobile number verification | P1 |
| Two-Factor Authentication | Enhanced security option | P2 |
| SSO Integration | Enterprise/Corporate accounts | P3 |

#### 2.1.2 User Profiles
- **Multiple Profiles**: Up to 5 profiles per account
- **Kids Profile**: Restricted content with parental controls
- **Profile Customization**: Avatar, name, language preference
- **Watch History**: Per-profile viewing history
- **Profile PIN**: Optional PIN lock for mature content

#### 2.1.3 Subscription Management
| Tier | Screens | Quality | Price/Month | Features |
|------|---------|---------|-------------|----------|
| Basic | 1 | 720p | $7.99 | Mobile only |
| Standard | 2 | 1080p | $12.99 | TV + Mobile |
| Premium | 4 | 4K HDR | $19.99 | All devices + Downloads |
| Family | 6 | 4K HDR | $24.99 | Premium + 6 profiles |

---

### 2.2 Content Management

#### 2.2.1 Content Types
```
├── Movies
│   ├── Hollywood
│   ├── Bollywood
│   ├── Regional
│   └── International
├── TV Shows
│   ├── Web Series
│   ├── Daily Soaps
│   └── Reality Shows
├── Originals (StreamVerse Exclusives)
├── Documentaries
├── Kids & Family
├── Sports (Live & Recorded)
└── Music Videos
```

#### 2.2.2 Content Metadata
| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | String | Yes | Content title |
| description | Text | Yes | Synopsis (max 500 chars) |
| genre | Array | Yes | Up to 3 genres |
| release_year | Integer | Yes | Year of release |
| duration | Integer | Yes | Length in minutes |
| rating | Enum | Yes | U, UA, A, R |
| language | Array | Yes | Audio languages |
| subtitles | Array | No | Available subtitles |
| cast | Array | Yes | Actors/Directors |
| thumbnail | URL | Yes | Poster image |
| trailer | URL | No | Trailer video URL |
| tags | Array | No | Searchable tags |

#### 2.2.3 Content Ingestion Pipeline
```
Upload → Transcoding → Quality Check → Metadata → CDN → Live
   │         │              │            │         │
   └─ S3 ────┴─ MediaConvert ┴─ Manual ───┴─ DynamoDB┴─ CloudFront
```

---

### 2.3 Video Player

#### 2.3.1 Core Features
| Feature | Description | Priority |
|---------|-------------|----------|
| Adaptive Bitrate | Auto quality based on bandwidth | P0 |
| Multiple Quality | 480p, 720p, 1080p, 4K | P0 |
| Subtitles/CC | Multiple language support | P0 |
| Audio Tracks | Multiple language audio | P0 |
| Playback Speed | 0.5x to 2x | P1 |
| Skip Intro | Auto-skip opening credits | P1 |
| Skip Recap | Skip "previously on" | P1 |
| Picture-in-Picture | Floating player | P2 |
| Chromecast/AirPlay | Cast to TV | P1 |

#### 2.3.2 Playback Controls
- Play/Pause
- Seek (forward/backward 10 seconds)
- Volume control
- Fullscreen toggle
- Quality selection
- Subtitle toggle
- Audio track selection
- Episode selector (for series)

#### 2.3.3 Smart Features
- **Continue Watching**: Resume from last position
- **Auto-Play Next**: Auto-play next episode
- **Binge Mode**: Reduced credits, faster transitions
- **Sleep Timer**: Auto-stop after X episodes/hours

---

### 2.4 Search & Discovery

#### 2.4.1 Search Capabilities
| Type | Description |
|------|-------------|
| Text Search | Title, cast, director search |
| Voice Search | Speech-to-text search |
| Filter Search | By genre, year, rating, language |
| Smart Search | Typo tolerance, synonyms |

#### 2.4.2 Content Discovery
- **Home Feed**: Personalized recommendations
- **Trending**: Popular content in region
- **New Releases**: Recently added content
- **Top 10**: Most watched this week
- **Because You Watched**: Similar content suggestions
- **Genre Rows**: Action, Comedy, Drama, etc.
- **Mood-based**: Feel-good, Thriller, Romantic

#### 2.4.3 Recommendation Engine
```
Input Signals:
├── Watch History
├── Search Queries
├── Time Spent on Thumbnails
├── Ratings/Reviews
├── Similar User Preferences
└── Content Metadata

Algorithm:
├── Collaborative Filtering
├── Content-Based Filtering
└── Hybrid Approach (Matrix Factorization + Deep Learning)

Output:
├── Personalized Home Feed
├── "More Like This" Suggestions
└── Email/Push Recommendations
```

---

### 2.5 Downloads & Offline Viewing

#### 2.5.1 Download Features
| Feature | Description |
|---------|-------------|
| Quality Selection | Choose download quality |
| Storage Management | View/clear downloaded content |
| Auto-Downloads | Download next episodes automatically |
| Smart Downloads | Delete watched, download new |
| Expiry | Downloads expire after 30 days |
| Offline Limit | Max 25 titles at a time |

#### 2.5.2 DRM Protection
- **Widevine**: Android, Chrome
- **FairPlay**: iOS, Safari
- **PlayReady**: Windows, Edge

---

### 2.6 Social Features

#### 2.6.1 User Interactions
| Feature | Description | Priority |
|---------|-------------|----------|
| Ratings | 1-5 star ratings | P1 |
| Reviews | Text reviews with spoiler tags | P2 |
| Watchlist | Save for later | P0 |
| Share | Share to social media | P1 |
| Watch Party | Synchronized viewing with friends | P2 |

#### 2.6.2 Watch Party
- Real-time synchronized playback
- Voice/Video chat integration
- Up to 10 participants
- Host controls (play/pause/seek)
- Chat with reactions

---

### 2.7 Advertisement Management System

#### 2.7.1 Ad-Supported Tiers
| Tier | Ad Frequency | Ad Duration | Skip Option |
|------|--------------|-------------|-------------|
| Free | Every 15 min | 30-60 sec | After 5 sec |
| Basic (Discounted) | Every 30 min | 15-30 sec | After 5 sec |
| Standard+ | Ad-free | - | - |
| Premium | Ad-free | - | - |

#### 2.7.2 Ad Types

```
┌─────────────────────────────────────────────────────────────────┐
│                      AD FORMATS SUPPORTED                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐   │
│  │   Pre-Roll Ads   │  │   Mid-Roll Ads   │  │ Post-Roll Ads│   │
│  │                  │  │                  │  │              │   │
│  │ • Before content │  │ • During content │  │ • After      │   │
│  │ • 15-30 seconds  │  │ • 15-60 seconds  │  │   content    │   │
│  │ • Skippable      │  │ • Natural breaks │  │ • 15-30 sec  │   │
│  │   after 5s       │  │ • Non-skippable  │  │ • Skippable  │   │
│  └──────────────────┘  └──────────────────┘  └──────────────┘   │
│                                                                 │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐   │
│  │  Banner/Overlay  │  │  Pause Screen    │  │ Binge Ads    │   │
│  │                  │  │                  │  │              │   │
│  │ • L-shaped       │  │ • When user      │  │ • Between    │   │
│  │   banners        │  │   pauses video   │  │   episodes   │   │
│  │ • Lower thirds   │  │ • Interactive    │  │ • Sponsorship│   │
│  │ • Clickable      │  │ • Dismissable    │  │ • 15-30 sec  │   │
│  └──────────────────┘  └──────────────────┘  └──────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### 2.7.3 Ad Insertion Methods

| Method | Description | Use Case |
|--------|-------------|----------|
| **SSAI** (Server-Side) | Ads stitched into video stream | Primary - prevents ad blockers |
| **CSAI** (Client-Side) | Ads inserted by player | Fallback, more targeting options |
| **DAI** (Dynamic) | Real-time ad decisions | Personalized ads |

#### 2.7.4 Ad Targeting Parameters

```
Targeting Dimensions:
├── Demographics
│   ├── Age group
│   ├── Gender
│   ├── Location (Country/State/City)
│   └── Language preference
│
├── Behavioral
│   ├── Watch history genres
│   ├── Time of day viewing
│   ├── Device type
│   ├── Binge watcher vs casual
│   └── Content completion rate
│
├── Contextual
│   ├── Current content genre
│   ├── Content rating (U/UA/A)
│   ├── Content mood/theme
│   └── Live vs On-demand
│
└── Custom Audiences
    ├── Retargeting (cart abandoners)
    ├── Lookalike audiences
    └── First-party data segments
```

#### 2.7.5 Ad Management Dashboard (Admin)

| Feature | Description |
|---------|-------------|
| **Campaign Management** | Create, edit, pause, delete ad campaigns |
| **Creative Upload** | Upload video ads (MP4, WebM), banners (JPG, PNG, GIF) |
| **Scheduling** | Set start/end dates, dayparting, frequency caps |
| **Targeting** | Configure audience targeting rules |
| **Budget Management** | Set daily/total budgets, bid amounts |
| **A/B Testing** | Test multiple creatives, measure performance |
| **Reporting** | Impressions, clicks, CTR, completion rate, revenue |
| **Brand Safety** | Block list for content categories |

#### 2.7.6 Ad Serving Flow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Player    │────▶│  Ad Server  │────▶│ Ad Decision │
│  Requests   │     │  (VAST/VMAP)│     │   Engine    │
└─────────────┘     └─────────────┘     └──────┬──────┘
                                               │
                    ┌──────────────────────────┘
                    │
                    ▼
┌─────────────────────────────────────────────────────────────┐
│                    AD DECISION ENGINE                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. Receive ad request with user context                    │
│  2. Query eligible campaigns (budget, schedule, targeting)  │
│  3. Apply frequency capping rules                           │
│  4. Run real-time auction (if RTB enabled)                  │
│  5. Select winning ad(s)                                    │
│  6. Return VAST/VMAP response                               │
│                                                             │
└─────────────────────────────────────────────────────────────┘
                    │
                    ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Ad Player  │────▶│  Tracking   │────▶│  Analytics  │
│  Renders    │     │  Pixels     │     │  Dashboard  │
└─────────────┘     └─────────────┘     └─────────────┘
```

#### 2.7.7 Ad Pods Configuration

| Content Duration | Pre-Roll | Mid-Roll Breaks | Post-Roll |
|------------------|----------|-----------------|-----------|
| < 10 min | 1 ad (15s) | None | Optional |
| 10-30 min | 1 ad (30s) | 1 break (2 ads) | Optional |
| 30-60 min | 1 ad (30s) | 2 breaks (2-3 ads each) | 1 ad |
| > 60 min (Movie) | 1 ad (30s) | 3-4 breaks (2-3 ads each) | 1 ad |
| Live Sports | 1 ad | Natural breaks (timeouts) | 1 ad |

#### 2.7.8 Ad Metrics & KPIs

| Metric | Description | Target |
|--------|-------------|--------|
| **Fill Rate** | % of ad requests filled | > 95% |
| **Completion Rate** | % of ads watched to end | > 70% |
| **Click-Through Rate** | Clicks / Impressions | > 1% |
| **Viewability** | % of ads actually seen | > 90% |
| **eCPM** | Effective cost per 1000 impressions | $15-30 |
| **Ad Load Time** | Time to start ad playback | < 1 sec |
| **Error Rate** | Failed ad plays | < 2% |

#### 2.7.9 Advertiser Self-Serve Portal

```
┌─────────────────────────────────────────────────────────────────┐
│                    ADVERTISER PORTAL                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │
│  │  Dashboard  │  │  Campaigns  │  │       Creatives         │  │
│  │             │  │             │  │                         │  │
│  │ • Spend     │  │ • Create    │  │ • Upload video/banner   │  │
│  │ • Impressions│ │ • Edit      │  │ • Preview               │  │
│  │ • CTR       │  │ • Duplicate │  │ • Approval status       │  │
│  │ • Revenue   │  │ • Archive   │  │ • A/B variants          │  │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘  │
│                                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │
│  │  Audiences  │  │   Billing   │  │       Reports           │  │
│  │             │  │             │  │                         │  │
│  │ • Create    │  │ • Add funds │  │ • Performance           │  │
│  │   segments  │  │ • Invoices  │  │ • Audience insights     │  │
│  │ • Import    │  │ • Payment   │  │ • Export CSV/PDF        │  │
│  │ • Lookalike │  │   history   │  │ • Schedule reports      │  │
│  └─────────────┘  └─────────────┘  └─────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

#### 2.7.10 Ad Database Schema

```sql
-- Advertisers
CREATE TABLE advertisers (
    id UUID PRIMARY KEY,
    company_name VARCHAR(255) NOT NULL,
    contact_email VARCHAR(255) NOT NULL,
    billing_address TEXT,
    balance DECIMAL(10,2) DEFAULT 0,
    status VARCHAR(20) DEFAULT 'active',
    created_at TIMESTAMP DEFAULT NOW()
);

-- Campaigns
CREATE TABLE ad_campaigns (
    id UUID PRIMARY KEY,
    advertiser_id UUID REFERENCES advertisers(id),
    name VARCHAR(255) NOT NULL,
    objective VARCHAR(50), -- awareness, traffic, conversions
    budget_total DECIMAL(10,2),
    budget_daily DECIMAL(10,2),
    bid_amount DECIMAL(10,4),
    bid_type VARCHAR(20), -- CPM, CPC, CPCV
    start_date TIMESTAMP,
    end_date TIMESTAMP,
    status VARCHAR(20) DEFAULT 'draft',
    created_at TIMESTAMP DEFAULT NOW()
);

-- Ad Creatives
CREATE TABLE ad_creatives (
    id UUID PRIMARY KEY,
    campaign_id UUID REFERENCES ad_campaigns(id),
    name VARCHAR(255),
    type VARCHAR(20), -- video, banner, overlay
    duration_seconds INTEGER,
    file_url VARCHAR(500),
    thumbnail_url VARCHAR(500),
    click_url VARCHAR(500),
    approval_status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT NOW()
);

-- Targeting Rules
CREATE TABLE ad_targeting (
    id UUID PRIMARY KEY,
    campaign_id UUID REFERENCES ad_campaigns(id),
    target_type VARCHAR(50), -- geo, age, gender, genre, device
    target_value TEXT, -- JSON array of values
    include BOOLEAN DEFAULT TRUE
);

-- Ad Impressions (for reporting)
CREATE TABLE ad_impressions (
    id UUID PRIMARY KEY,
    creative_id UUID REFERENCES ad_creatives(id),
    user_id UUID,
    profile_id UUID,
    content_id UUID,
    device_type VARCHAR(20),
    geo_country VARCHAR(10),
    impression_at TIMESTAMP DEFAULT NOW(),
    quartile_reached INTEGER, -- 0, 25, 50, 75, 100
    clicked BOOLEAN DEFAULT FALSE,
    skipped BOOLEAN DEFAULT FALSE,
    skip_time_seconds INTEGER
);

-- Frequency Capping
CREATE TABLE ad_frequency_caps (
    id UUID PRIMARY KEY,
    campaign_id UUID REFERENCES ad_campaigns(id),
    cap_type VARCHAR(20), -- per_user, per_session, per_day
    max_impressions INTEGER,
    time_window_hours INTEGER
);
```

#### 2.7.11 Integration with Ad Networks

| Network | Type | Use Case |
|---------|------|----------|
| **Google Ad Manager** | Primary | Programmatic, backfill |
| **SpotX** | Video SSP | Premium video inventory |
| **FreeWheel** | TV/OTT | Connected TV monetization |
| **Amazon TAM** | Header Bidding | Unified auction |
| **Direct Deals** | Custom | Premium advertisers |

#### 2.7.12 Ad-Related User Settings

| Setting | Options | Default |
|---------|---------|---------|
| Ad Personalization | On / Off | On |
| Interest-Based Ads | On / Off | On |
| Ad Volume | Same as content / Louder / Softer | Same |
| Ad Preferences | Select interests to see relevant ads | - |
| Opt-out of tracking | For GDPR/CCPA compliance | Off |

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

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CLIENT LAYER                                    │
├─────────────────────────────────────────────────────────────────────────────┤
│   ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────────────┐   │
│   │   Web   │  │   iOS   │  │ Android │  │Smart TV │  │ Fire TV/Roku    │   │
│   │ React.js│  │  Swift  │  │ Kotlin  │  │ Tizen/  │  │                 │   │
│   │         │  │         │  │         │  │ WebOS   │  │                 │   │
│   └────┬────┘  └────┬────┘  └────┬────┘  └────┬────┘  └────────┬────────┘   │
└────────┼────────────┼────────────┼────────────┼─────────────────┼───────────┘
         │            │            │            │                 │
         └────────────┴────────────┴────────────┴─────────────────┘
                                   │
                                   ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                              API GATEWAY                                     │
│                         (AWS API Gateway / Kong)                             │
│         Rate Limiting │ Authentication │ Request Routing │ Caching          │
└─────────────────────────────────────────────────────────────────────────────┘
                                   │
         ┌─────────────────────────┼─────────────────────────┐
         │                         │                         │
         ▼                         ▼                         ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────────────────┐
│  User Service   │    │ Content Service │    │    Streaming Service        │
│                 │    │                 │    │                             │
│ • Auth          │    │ • Catalog       │    │ • Video Delivery            │
│ • Profiles      │    │ • Metadata      │    │ • Adaptive Bitrate          │
│ • Subscriptions │    │ • Search        │    │ • DRM License               │
│ • Preferences   │    │ • Recommendations│   │ • CDN Origin                │
└────────┬────────┘    └────────┬────────┘    └──────────────┬──────────────┘
         │                      │                            │
         ▼                      ▼                            ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────────────────┐
│    PostgreSQL   │    │  Elasticsearch  │    │      AWS CloudFront         │
│    (Users DB)   │    │  (Search Index) │    │         (CDN)               │
└─────────────────┘    └─────────────────┘    └──────────────┬──────────────┘
                                                             │
         ┌───────────────────────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           STORAGE LAYER                                      │
├─────────────────────────────────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────┐ │
│  │  Amazon S3  │  │   DynamoDB  │  │    Redis    │  │   AWS MediaConvert  │ │
│  │  (Videos)   │  │  (Metadata) │  │   (Cache)   │  │   (Transcoding)     │ │
│  └─────────────┘  └─────────────┘  └─────────────┘  └─────────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Microservices Architecture

| Service | Responsibility | Tech Stack | Database |
|---------|----------------|------------|----------|
| **User Service** | Auth, Profiles, Subscriptions | Node.js/Express | PostgreSQL |
| **Content Service** | Catalog, Metadata, Search | Python/FastAPI | MongoDB + Elasticsearch |
| **Streaming Service** | Video delivery, DRM | Go | Redis |
| **Recommendation Service** | ML-based suggestions | Python/TensorFlow | Redis + S3 |
| **Payment Service** | Billing, Subscriptions | Node.js | PostgreSQL |
| **Notification Service** | Push, Email, SMS | Node.js | Redis + SQS |
| **Analytics Service** | Tracking, Reporting | Python/Spark | ClickHouse |
| **Admin Service** | CMS, Content Management | Node.js/React | PostgreSQL |

### 3.3 Video Delivery Pipeline

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   Content    │     │  Transcoding │     │     CDN      │
│   Upload     │────▶│   Pipeline   │────▶│  Distribution│
└──────────────┘     └──────────────┘     └──────────────┘
       │                    │                    │
       ▼                    ▼                    ▼
  ┌─────────┐        ┌───────────┐        ┌───────────┐
  │  S3     │        │ Multiple  │        │ CloudFront│
  │ Ingest  │        │ Renditions│        │ Edge Nodes│
  │ Bucket  │        │           │        │ Worldwide │
  └─────────┘        │ • 4K      │        └───────────┘
                     │ • 1080p   │
                     │ • 720p    │
                     │ • 480p    │
                     │ • 360p    │
                     │ + HLS/DASH│
                     └───────────┘

Transcoding Output Formats:
├── HLS (HTTP Live Streaming) - Apple devices
├── DASH (Dynamic Adaptive Streaming) - Android/Web
└── Smooth Streaming - Windows devices
```

### 3.4 Database Schema (Core Tables)

```sql
-- Users
CREATE TABLE users (
    id UUID PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255),
    phone VARCHAR(20),
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Profiles
CREATE TABLE profiles (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    name VARCHAR(100) NOT NULL,
    avatar_url VARCHAR(500),
    is_kids BOOLEAN DEFAULT FALSE,
    pin VARCHAR(4),
    language VARCHAR(10) DEFAULT 'en',
    maturity_level VARCHAR(20) DEFAULT 'adult'
);

-- Subscriptions
CREATE TABLE subscriptions (
    id UUID PRIMARY KEY,
    user_id UUID REFERENCES users(id),
    plan_id VARCHAR(50) NOT NULL,
    status VARCHAR(20) NOT NULL, -- active, cancelled, expired
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    payment_method_id UUID,
    auto_renew BOOLEAN DEFAULT TRUE
);

-- Content
CREATE TABLE content (
    id UUID PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    description TEXT,
    type VARCHAR(20) NOT NULL, -- movie, series, episode
    parent_id UUID REFERENCES content(id), -- for episodes
    season_number INTEGER,
    episode_number INTEGER,
    duration_minutes INTEGER,
    release_year INTEGER,
    maturity_rating VARCHAR(10),
    thumbnail_url VARCHAR(500),
    trailer_url VARCHAR(500),
    created_at TIMESTAMP DEFAULT NOW()
);

-- Watch History
CREATE TABLE watch_history (
    id UUID PRIMARY KEY,
    profile_id UUID REFERENCES profiles(id),
    content_id UUID REFERENCES content(id),
    progress_seconds INTEGER DEFAULT 0,
    completed BOOLEAN DEFAULT FALSE,
    watched_at TIMESTAMP DEFAULT NOW()
);

-- Watchlist
CREATE TABLE watchlist (
    id UUID PRIMARY KEY,
    profile_id UUID REFERENCES profiles(id),
    content_id UUID REFERENCES content(id),
    added_at TIMESTAMP DEFAULT NOW(),
    UNIQUE(profile_id, content_id)
);
```

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

Daily Ad Impressions: 5M × 2hr × 4ads = 40M impressions
Monthly Ad Impressions: 40M × 30 = 1.2B impressions

At $20 eCPM (average):
Monthly Ad Revenue = (1.2B / 1000) × $20 = $24 Million
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
├── Limited content library
├── 720p max quality
├── 2-3 ads per hour
└── No downloads

Paid Tiers:
├── Basic: $7.99/mo - 1 screen, 720p, Mobile only
├── Standard: $12.99/mo - 2 screens, 1080p, All devices
├── Premium: $19.99/mo - 4 screens, 4K HDR, Downloads
└── Annual: 2 months free on yearly subscription
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

**Document Version**: 1.0
**Last Updated**: February 2026
**Author**: StreamVerse Product Team
