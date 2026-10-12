# AskMy.pink Technical Roadmap

## Platform Requirements
- **Mobile-first**: Native or cross-platform (Flutter/React Native) iOS & Android apps
- **Voice**: Easy voice input for questions (speech-to-text + intent parsing)
- **Notifications**: Rich push notifications that open directly into the AI-enhanced chat
- **AI**:
  - Research agent (web/search + location APIs)
  - Recommendation engine that learns habits (preferences, past decisions, joint patterns)
  - Action layer (integrations for reservations/booking)
- **Social graph**: Lightweight contacts/tagging system (not full public social network initially)

## Core User Flow
1. User opens app or uses voice shortcut
2. Speaks/types: "@wife where do you want to go eat around main street"
3. AI parses intent, location, tagged user
4. Runs research + personalization
5. Delivers enriched message to tagged person's app/notification
6. Recipient can reply, accept suggestion, or trigger booking

## Phases
**Phase 1 – MVP (0–3 months)**
- Basic tagging + chat delivery
- Simple AI research (location-based restaurant/search)
- Voice input
- Push notifications
- Manual habit notes

**Phase 2 – Intelligence (3–6 months)**
- Habit learning from interactions
- Better personalization
- Booking integrations (partner APIs)
- Family groups

**Phase 3 – Scale (6–12 months)**
- Advanced multi-person decisions
- Broader categories (travel, shopping, activities)
- Monetization features
- Web companion / site at askmy.pink

## Stack Suggestions
- Mobile: Flutter or React Native
- Backend: Node/Python + PostgreSQL
- AI: LLM orchestration + RAG + user preference embeddings
- Notifications: Firebase / APNs
- Repo: This GitHub repository for planning; product code in separate repo later

All work and updates saved here and notified via Slack.
