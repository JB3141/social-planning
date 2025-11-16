# Product Requirements Document: Social Planning MVP

**Created:** 2025-11-16
**Status:** Planning
**Target:** MVP Launch

---

## Vision

**The Inverse of Partiful**

Partiful solves: "I know when/where/what, I just need to invite people"

Social Planning solves: "I know WHO I want to see, but when/where/what needs coordination"

**Core Problem:** Most social plans don't happen because of the managerial overhead of coordinating schedules, finding locations, and going back and forth on details.

**Solution:** Meet people where they already make plans (iMessage) and handle the coordination automatically.

---

## User Story

### Primary Flow

**You:**
1. Haven't seen Shelby in a while, want to catch up
2. Open Social Planning web app on phone
3. Select "Shelby" from friends
4. Tap "Generate Options"
5. Claude suggests 3 smart options based on:
   - Your calendar availability
   - Both locations (Oakland + Mission SF)
   - Shared preferences (you both like brunch, coffee)
   - Time patterns (Shelby prefers weekend mornings)
6. Review options, maybe tweak one
7. Tap "Copy for iMessage"
8. Paste into iMessage thread with Shelby, send

**Shelby:**
1. Receives iMessage with 3 tappable options:
   ```
   Hey! Want to catch up? Pick what works:

   🍸 Tuesday 6pm → plan.app/abc
   ☕ Saturday 10am → plan.app/def
   🍽️ Sunday 7pm → plan.app/ghi
   ```
2. Taps "Saturday 10am" link
3. Sees confirmation page:
   - Date/time/activity/location details
   - Map preview
   - "Add to Calendar" button
4. Taps confirm
5. Event added to both calendars automatically

**Result:** Plan made in ~30 seconds with zero back-and-forth.

---

## Core Features (MVP)

### 1. Planning Interface (Web App)

**Mobile-responsive web application for plan creation**

**Features:**
- Friend selector dropdown
- "Generate Options" using Claude API
- 3 option slots (editable)
- Each option includes:
  - Day/time
  - Activity type (drinks, coffee, dinner, etc.)
  - Venue name + neighborhood
- Preview of iMessage message
- One-tap copy to clipboard

**AI Generation (Claude API):**
- Analyzes your Google Calendar for free slots
- References friend preferences from config
- Suggests appropriate times based on:
  - Day of week patterns
  - Typical meeting times
  - Location midpoints
  - Activity preferences
- Suggests specific venues (from preferences or future Yelp/Google Places integration)

### 2. Confirmation Flow

**Simple, mobile-optimized confirmation page**

**URL Format:**
- Pretty URLs: `plan.app/tuesday-6pm-drinks` or
- Short codes with emoji: `🍸 Tue 6pm → plan.app/abc`

**Confirmation Page Shows:**
- Full event details:
  - Date/time (formatted nicely)
  - Activity emoji + name
  - Venue name + address
  - Map preview or link
- Who it's with (your name)
- Clear "Add to Calendar" CTA button
- Success state after confirmation

**After Confirmation:**
- Creates event in your Google Calendar
- Sends .ics calendar invite to friend's email
- Shows success message: "Done! Check your calendar 🎉"
- Option to add to Apple Calendar / Google Calendar directly

### 3. Calendar Integration

**Phase 1 (MVP):**
- You grant Google Calendar API access (one-time OAuth)
- App reads your free/busy to suggest times
- App creates events in your calendar
- Sends .ics email invites to friends

**Phase 2 (Future):**
- Friends can optionally grant calendar access
- Direct calendar event creation (no email invite needed)
- Automatic conflict detection

### 4. Friend & Preference Management

**Initial Approach: JSON Config File**

Location: `~/.social-planner/config.json` or in database

```json
{
  "user": {
    "name": "Jonathan",
    "email": "you@example.com",
    "calendar_id": "your-google-calendar-id",
    "location": {
      "home": "Oakland, CA",
      "work": "SF Financial District"
    },
    "preferences": {
      "willing_to_travel": "30min",
      "preferred_neighborhoods": ["Rockridge", "Temescal", "Hayes Valley"],
      "preferred_times": {
        "weekday": "after 6pm",
        "weekend": "flexible"
      }
    }
  },
  "friends": {
    "Shelby": {
      "email": "shelby@example.com",
      "phone": "+1234567890",
      "location": {
        "home": "Mission, SF"
      },
      "preferences": {
        "preferred_times": {
          "weekday": "after 7pm",
          "weekend": "morning preferred"
        },
        "preferred_neighborhoods": ["Mission", "Castro", "Downtown Oakland"],
        "likes": ["coffee", "brunch", "new restaurants"]
      }
    },
    "Harry": {
      "email": "harry@example.com",
      "location": {
        "home": "Berkeley, CA"
      },
      "preferences": {
        "likes": ["breweries", "hiking", "dinner"],
        "preferred_neighborhoods": ["North Berkeley", "Rockridge"]
      }
    }
  },
  "groups": {
    "Harry + Shelby": {
      "usual_locations": ["Rockridge", "Downtown Oakland"],
      "usual_activities": ["dinner", "brunch"],
      "notes": "We usually do monthly dinners, like trying new places"
    }
  },
  "venues": {
    "favorites": [
      {
        "name": "Local Edition",
        "type": "bar",
        "location": "Hayes Valley, SF",
        "notes": "Great for weeknight drinks"
      },
      {
        "name": "Tartine",
        "type": "cafe",
        "location": "Mission, SF",
        "notes": "Weekend brunch spot"
      }
    ]
  }
}
```

**Future: Web UI for preference management**

---

## Technical Architecture

### Tech Stack

**Frontend:**
- Next.js (or simple HTML/CSS/JS for faster MVP)
- TailwindCSS for styling
- Mobile-first responsive design
- Copy-to-clipboard API

**Backend:**
- Node.js + Express
- PostgreSQL database
- RESTful API

**APIs & Services:**
- Anthropic Claude API (you have Claude Max subscription)
- Google Calendar API (OAuth 2.0)
- Future: Google Places API for venue suggestions

**Hosting:**
- Vercel (frontend + serverless functions)
- Railway or Render (backend + database)
- Domain: `plan.app` or `socialplan.app` (to be decided)

**Development:**
- Git for version control
- GitHub for repository
- Compounding engineering workflow (PRD → Work → Review → Learn)

### Data Models

**Plan Proposal:**
```javascript
{
  id: "abc123",
  creator_id: "user_id",
  friend_name: "Shelby",
  friend_email: "shelby@example.com",
  options: [
    {
      option_id: "1",
      datetime: "2024-11-19T18:00:00-08:00",
      activity_type: "drinks",
      activity_emoji: "🍸",
      venue_name: "Local Edition",
      venue_address: "691 Market St, SF",
      neighborhood: "Hayes Valley",
      notes: "Midpoint between you both"
    },
    // ... 2 more options
  ],
  selected_option: null, // becomes option_id when confirmed
  status: "pending", // pending | confirmed | expired
  created_at: "2024-11-16T10:00:00Z",
  confirmed_at: null,
  calendar_event_id: null
}
```

**User Preferences:**
```javascript
{
  user_id: "user_id",
  name: "Jonathan",
  email: "you@example.com",
  google_calendar_id: "calendar_id",
  location: {...},
  preferences: {...}
}
```

**Friend:**
```javascript
{
  friend_id: "friend_id",
  user_id: "user_id", // who this friend belongs to
  name: "Shelby",
  email: "shelby@example.com",
  phone: "+1234567890",
  location: {...},
  preferences: {...}
}
```

### API Endpoints

**Frontend (Web App):**
```
GET  /                          # Landing page
GET  /plan                      # Planning interface
POST /api/generate-options      # Generate 3 options using Claude
POST /api/create-plan           # Save plan, get shareable links
GET  /c/:planId/:optionId       # Confirmation page
POST /api/confirm-plan          # Confirm selected option, create calendar event
```

**Backend:**
```
POST /api/auth/google           # OAuth for Google Calendar
GET  /api/calendar/availability # Check user's calendar free/busy
POST /api/suggestions           # Claude-powered suggestions
POST /api/calendar/create-event # Create calendar event
POST /api/send-invite           # Send .ics email
```

### Security & Privacy

- OAuth 2.0 for Google Calendar (standard flow)
- User data encrypted at rest
- HTTPS only
- No sharing of calendar details beyond free/busy
- Friends don't need accounts (email-based invites)
- Option to revoke calendar access anytime

---

## Implementation Phases

### Phase 1: Core Flow (Week 1-2)
**Goal: Working end-to-end flow with manual options**

**Tasks:**
1. Set up project structure (Node.js + Express)
2. Create PostgreSQL database + models
3. Build simple web form:
   - Friend selector (dropdown from config)
   - 3 manual option inputs
   - Generate shareable links
   - Copy to clipboard
4. Build confirmation page:
   - Display option details
   - Confirm button
   - Success state
5. Implement basic calendar event creation:
   - Create event in your Google Calendar
   - Generate .ics file
   - Send email with .ics attachment

**Success Criteria:**
- Can manually enter 3 options
- Copy formatted message for iMessage
- Friend can tap link → confirm
- Event appears in your calendar
- Friend receives calendar invite email

### Phase 2: Smart Suggestions (Week 3)
**Goal: AI-powered option generation**

**Tasks:**
1. Integrate Anthropic Claude API
2. Implement calendar availability checker (Google Calendar API)
3. Build suggestion algorithm:
   - Parse user + friend preferences
   - Check calendar for free slots
   - Generate contextual suggestions (time + activity + venue)
4. Add "Generate Options" button to UI
5. Allow editing of AI suggestions before sending

**Success Criteria:**
- Can tap "Generate" and get 3 smart suggestions
- Suggestions respect calendar availability
- Suggestions match friend preferences
- Can still manually edit before sending

### Phase 3: Polish & Enhancement (Week 4)
**Goal: Improved UX and reliability**

**Tasks:**
1. Add link preview metadata (Open Graph tags)
2. Improve confirmation page design
3. Add map preview/link to venue
4. Implement plan expiration (auto-expire after 1 week)
5. Add basic analytics (plans created, confirmation rate)
6. Error handling and loading states
7. Mobile UX optimization

**Success Criteria:**
- Links show nice previews in iMessage
- Confirmation page looks polished
- Plans expire appropriately
- Graceful error handling

### Phase 4: Future Enhancements

**Not in MVP, but documented for later:**

1. **Recurring Plans**
   - "Monthly brunch with Shelby"
   - Auto-suggest next occurrence
   - Track cadence ("you haven't seen Harry in 6 weeks")

2. **Group Plans**
   - Plan with 2+ friends
   - Find overlapping availability
   - Group preferences

3. **Learning & Intelligence**
   - Learn from past choices
   - Analyze iMessage history for preferences (with permission)
   - Venue recommendation improvements
   - Time preference learning

4. **Calendar Improvements**
   - Friend calendar access (optional)
   - Direct calendar creation (no email)
   - Automatic conflict detection
   - Travel time awareness

5. **iMessage Extension**
   - Native iMessage app
   - Interactive messages
   - No need to leave iMessage

6. **Preference Management UI**
   - Web UI for managing friends
   - Edit preferences visually
   - Import from contacts

---

## User Experience Details

### iMessage Message Format

**Option 1: Emoji + Short Links**
```
Hey! Want to catch up? Pick what works:

🍸 Tuesday 6pm drinks
plan.app/abc

☕ Saturday 10am coffee
plan.app/def

🍽️ Sunday 7pm dinner
plan.app/ghi
```

**Option 2: Inline Links**
```
Hey! Want to catch up?

Tuesday 6pm drinks (plan.app/abc)
Saturday 10am coffee (plan.app/def)
Sunday 7pm dinner (plan.app/ghi)
```

**Option 3: Rich Preview (if link previews work)**
Just URLs, let iMessage generate rich previews from Open Graph metadata.

**Decision: Start with Option 1, test what works best in real usage**

### Confirmation Page UX

**Layout:**
```
┌─────────────────────────────┐
│ Social Planning             │
│                             │
│  📅 Tuesday, November 19    │
│      6:00 PM                │
│                             │
│  🍸 Drinks                  │
│                             │
│  📍 Local Edition           │
│     691 Market St           │
│     Hayes Valley, SF        │
│     [View Map]              │
│                             │
│  👤 With Jonathan           │
│                             │
│  ┌─────────────────────┐   │
│  │  ✅ Confirm Plan    │   │
│  └─────────────────────┘   │
│                             │
│  Or pick a different time   │
│  [Other options...]         │
└─────────────────────────────┘
```

**After Confirmation:**
```
┌─────────────────────────────┐
│  🎉 You're all set!         │
│                             │
│  Tuesday, Nov 19 at 6pm     │
│  Drinks at Local Edition    │
│                             │
│  ✅ Added to your calendar  │
│  ✅ Jonathan notified       │
│                             │
│  📧 Calendar invite sent to │
│     shelby@example.com      │
│                             │
│  See you there!             │
└─────────────────────────────┘
```

---

## Open Questions & Decisions Needed

### 1. Calendar Invite Mechanism

**Option A: Email .ics only (MVP)**
- Simpler implementation
- Friend clicks .ics in email → imports to calendar
- Works cross-platform

**Option B: Friend calendar access**
- Requires friend to grant access when confirming
- More seamless (no email step)
- Privacy concerns

**Decision: Start with Option A, add Option B later as enhancement**

### 2. Friend Data Storage

**For MVP:**
- JSON config file (manual editing)
- Quick to set up
- Easy to version control

**For Scale:**
- Database with web UI
- Import from contacts
- Share preferences between users (if social network)

**Decision: JSON for MVP, plan web UI for Phase 3**

### 3. URL Format

**Short & Pretty:**
- `plan.app/tue-6pm-drinks`
- More readable
- Longer URLs

**Short Codes:**
- `plan.app/abc123`
- Shorter
- Less descriptive

**Decision: Use short codes with descriptive text in message**

### 4. Domain Name

Options:
- `plan.app` (short, may be taken/expensive)
- `socialplan.app`
- `letsplan.app`
- `meetup.social`

**Decision: Research availability, decide before Phase 1**

### 5. Notification Strategy

**When friend confirms:**
- Email to you: "Shelby confirmed Saturday 10am coffee!"
- SMS notification (requires Twilio)
- Push notification (requires native app)

**Decision: Email notification for MVP, add SMS in Phase 3**

---

## Success Metrics

### MVP Success Criteria

**Qualitative:**
- Plans are easier to make than traditional texting
- Friends find confirmation flow intuitive
- You use it for real plans (not just testing)

**Quantitative:**
- 10 real plans created and confirmed
- <30 seconds to generate + send options
- >70% confirmation rate (friend picks an option)
- Zero failed calendar invites

### Future Metrics

- Plans created per week
- Time saved vs traditional coordination
- Confirmation rate by time of day
- Most popular activities/venues
- Recurring plan adoption

---

## Technical Risks & Mitigations

### Risk 1: Google Calendar API Complexity
**Mitigation:**
- Start with simple free/busy check
- Use well-documented libraries (googleapis npm package)
- Fallback to manual time entry if API fails

### Risk 2: iMessage Link Rendering
**Mitigation:**
- Test multiple URL formats
- Add Open Graph metadata for rich previews
- Have fallback text-only format

### Risk 3: Calendar Invite Delivery
**Mitigation:**
- Test .ics generation thoroughly
- Support multiple calendar providers
- Provide alternative download link

### Risk 4: Claude API Rate Limits
**Mitigation:**
- You have Claude Max (higher limits)
- Cache common suggestions
- Fallback to template-based suggestions

### Risk 5: Time Zone Handling
**Mitigation:**
- Use proper date libraries (date-fns, luxon)
- Store all times in ISO 8601 with timezone
- Display in user's local timezone

---

## Development Workflow

Following the Compounding Engineering approach:

1. **This PRD** - Detailed planning before coding
2. **Execution** - `/compounding-engineering:work tasks/PRD_social_planning_mvp.md`
3. **Review** - `/compounding-engineering:review` after each phase
4. **Learning Capture** - Update `CLAUDE.md` and create learning docs

**Key Patterns to Establish:**
- Calendar integration pattern
- AI suggestion generation pattern
- .ics file creation pattern
- URL shortening/routing pattern
- Mobile-responsive form patterns

**Documentation Updates:**
- Add calendar API patterns to `CLAUDE.md`
- Document AI prompting strategies
- Capture UX learnings
- Time estimates vs actuals

---

## Timeline Estimate

**Phase 1:** 8-12 hours (Core flow, manual options)
**Phase 2:** 6-8 hours (AI suggestions, calendar integration)
**Phase 3:** 4-6 hours (Polish, error handling)

**Total MVP:** ~20-25 hours of development

**Stretch Goal:** Working prototype this weekend, polished MVP in 2 weeks

---

## Appendix: Claude API Integration Details

### Suggestion Prompt Template

```
You are helping coordinate social plans. Generate 3 specific plan options.

Context:
- Planner: Jonathan (lives in Oakland, works in SF)
- Friend: Shelby (lives in Mission, SF)
- Relationship: Close friends who enjoy coffee, brunch, trying new restaurants
- Last met: 6 weeks ago
- Current date: Tuesday, November 16, 2024

Planner's availability this week:
- Thursday: Free after 6pm
- Saturday: Free all day
- Sunday: Free after 11am

Friend preferences:
- Prefers weekend mornings
- Likes Mission, Castro, Downtown Oakland neighborhoods
- Enjoys coffee shops and brunch spots

Generate 3 diverse options with this format:
1. Day/time, activity type, specific venue name + neighborhood

Optimize for:
- Convenient locations (midpoint or near one person)
- Appropriate timing (respect preferences)
- Variety (different days, activities, vibes)
```

**Expected Response:**
```
1. Thursday 6:30pm, drinks, Local Edition (Hayes Valley) - Midpoint between you, great cocktails
2. Saturday 10:00am, coffee, Sightglass (Mission) - Near Shelby, excellent coffee and pastries
3. Sunday 11:30am, brunch, Boot & Shoe (Oakland) - Near you, great pizza brunch, easy parking
```

### API Configuration

```javascript
import Anthropic from '@anthropic-ai/sdk';

const anthropic = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY,
});

async function generateSuggestions(plannerData, friendData, availability) {
  const message = await anthropic.messages.create({
    model: "claude-3-5-sonnet-20241022",
    max_tokens: 1024,
    messages: [{
      role: "user",
      content: buildPrompt(plannerData, friendData, availability)
    }]
  });

  return parseOptions(message.content);
}
```

---

## Next Steps

1. Review and finalize this PRD
2. Set up initial project structure
3. Begin Phase 1 implementation using `/compounding-engineering:work`
4. Test with real friends
5. Iterate based on feedback

---

**This PRD is a living document. Update as we learn and iterate.**
