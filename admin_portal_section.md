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

