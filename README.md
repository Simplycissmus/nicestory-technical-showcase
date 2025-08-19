# NiceStory.app - The Accidentally Complex Hobby Project

**Personal AI experiments that somehow became a production platform**

[![Live Thing](https://img.shields.io/badge/Live-nicestory.app-success)](https://nicestory.app)
[![My Portfolio](https://img.shields.io/badge/More_About_Me-aeberhard.ai-red)](https://aeberhard.ai)

## What Started as "How Hard Can AI Jailbreaking Be?"

Late nights reading about Chinese AI constraints, thinking "Pattern recognition should solve this easily." The jailbreaking worked, but my kids discovered the "security research stories" were actually good bedtime material. 

**Problem:** Kids want infinite stories. I want to sleep.  
**Solution:** Build a system that maintains narrative coherence across infinite branches.  
**Result:** 47 years of pattern obsession meets storytelling. A hobby project that accidentally became real.

---

## The Actual Technical Stuff (What Makes This Interesting)

### 1. Template Factory: User Idea → Immutable World

User drops ANY idea - the system builds a complete world:

```javascript
// Step 1: JSON Schema Analysis
{
  core: { mainIdea, targetAudience, narrativePotential },
  essentials: { 
    primaryGenre, mainConflict, toneAndMood,
    specificElements: { keyCharacters, importantSettings, uniqueConcepts }
  },
  structure: { suggestedAgeRating, complexity, branchingPotential }
}

// Step 2: 27 Function Refinement Pipeline
// Level 1: Foundation
['refine-age-and-genre']

// Level 2: Core Elements (after Level 1)  
['refine-background-info-v2', 'refine-text-content', 'refine-format-settings']

// Level 3: Advanced Features (after Level 2)
['refine-first-chapter', 'refine-narrative-style', 'refine-voice']
```

**The Insight:** Templates ≠ User Stories. Templates are universal (in `stories` table). User stories are personal (in `user_stories_v2`). Same template → infinite personal narratives.

### 2. The 3-AI-Call Architecture

From `generate-chapter-v1.js` - why one AI call isn't enough:

```
Call 1: Context Condensation
  - Take 50,000 tokens of story history
  - Compress to 2,000 tokens without losing narrative threads
  - "What happened so far that matters for what happens next?"

Call 2: Chapter Content Generation  
  - Use condensed context + user choice
  - Generate actual chapter content
  - Maintain character voices and world rules

Call 3: Choice Enhancement
  - Analyze generated chapter
  - Create 4 meaningful choices that branch the narrative
  - Ensure choices have actual consequences
```

**Why?** AI models have context windows. Stories don't.

### 3. Railway Internal Service Mesh

```
storyspark-model-management:8080    ← AI provider abstraction
ai-provider-factory:8080            ← Semantic routing ("fast" → best model)
storyspark-sse-service:8080         ← Real-time notifications
nicestory-pdf-generator             ← PDF export
nicestory-stripe-service            ← Payments
inngest-webhook                     ← Event orchestration (27 functions)
storyspark-content-service          ← SEO materialized views
```

**Railway's Internal DNS**: Services talk via `http://service-name:8080` - no internet latency, no external dependencies.

### 4. Cultural Intelligence & Auto-Classification

From `language-utils.js` - stories adapt to cultural context:

```javascript
const getLanguageConfig = (language) => {
  switch(language) {
    case 'de-CH':
      return {
        culturalContext: 'Swiss-German humor and directness',
        narrativeStyle: 'Detailed, methodical progression',
        characterTypes: 'Reserved but warm personalities'
      };
    case 'zh-CN':
      return {
        culturalContext: 'Collective harmony, family respect', 
        narrativeStyle: 'Circular storytelling, moral lessons',
        characterTypes: 'Duty-driven, ancestral wisdom'
      };
    // 9 languages with cultural intelligence
  }
};

// Auto-Classification (Fiction vs Non-Fiction)
const classifyNarrative = (analysis) => {
  if (analysis.uniqueConcepts.includes('magic')) return 'fiction';
  if (analysis.mainTheme.includes('historical')) return 'non-fiction';
  return 'fiction'; // Default
};
```

**The Magic:** User says "Harry Potter aber mit Emotionsmagie" → System knows it's German fantasy with emotional magic rules → Template locked with cultural German storytelling patterns.

### 5. Context Window Juggling

```javascript
// From story-metadata-loader.js - because AI has limits but stories don't
const buildHierarchicalContext = (chapterNumber, maxTokens = 4000) => {
  return {
    core: {                    // Always included (500 tokens)
      template: storyTemplate,
      protagonist: currentState,
      activeQuests: objectives
    },
    recent: {                  // Last 2 chapters (1000 tokens)
      summaries: lastChapters,
      choices: recentDecisions,
      activeCharacters: present
    },
    historical: {              // Compressed history (500 tokens)
      majorEvents: keyPlots,
      establishedFacts: worldState
    }
  };
};
```

**Smart Context Loading**: 80% fewer database queries through intelligent caching with 1-minute TTL.

---

## The "Nebenprodukte" (Side Projects That Became Infrastructure)

### AI Provider Factory
**Problem:** OpenAI goes down during peak usage.  
**Solution:** Database-driven routing with semantic aliases.

```javascript
// User requests 'fast' - system picks best available
await aiFactory.generate({
  model: 'fast',  // Could route to gpt-3.5, gemini-flash, or claude-haiku
  prompt: 'Generate chapter',
  project: 'nicestory'
});
```

**Real Innovation:** Configuration changes without code deploys. Provider outages handled automatically.

### SmartButton UX Pattern
**Problem:** 50+ async buttons all doing toast.loading(), toast.success(), toast.error().

```javascript
// Before: Repeated everywhere
const toastId = toast.loading('Processing...');
try {
  await action();
  toast.success('Done!', { id: toastId });
} catch {
  toast.error('Failed', { id: toastId });
}

// After: Write once, use everywhere
const buttonState = useSmartButton();
const handleAction = async () => {
  buttonState.setLoading();
  try {
    await action();
    buttonState.setSuccess('Done!');
  } catch {
    buttonState.setError('Failed');
  }
};
```

---

## The Interesting Bits (If You're Into This Stuff)

### Database Architecture Cleanup
**From:** 7 redundant status fields (published, is_visible, status, etc.)  
**To:** 2 unified fields (unified_status, unified_visibility)  
**Why:** Because debugging "which field is the source of truth?" at 2 AM sucks.

### SEO Event System
**Problem:** 100+ daily unnecessary database refreshes.  
**Solution:** PostgreSQL triggers + pg_notify for real-time updates.  
**Result:** 95% reduction in unnecessary operations.

### Metadata Migration (Currently Debugging)
Migrating 27 functions from manual metadata queries to centralized loader:
- 2/27 functions complete (refine-voice.js, generate-chapter-v1.js)
- 80% fewer database queries per function
- Consistent caching and error handling

---

## What's Actually Live

- **193 stories** in production (all public, no drafts)
- **27 specialized Inngest functions** processing stories
- **5-provider AI architecture** with automatic failover
- **PWA with service workers** for offline reading
- **Multi-language support** (but honestly, mostly tested with German and English)

## The Honest Tech Stack

### Why These Choices?
- **Netlify:** Best CI/CD for frontend
- **Railway:** No timeout limits (critical for AI workloads)
- **Supabase:** PostgreSQL with real-time, because I'm not managing my own DB
- **Inngest:** Event orchestration that actually works
- **Multiple AI Providers:** Because vendor lock-in is real

### What I Learned (47 Years of Pattern Recognition Applied)
1. **Side projects become main projects** - What started as AI curiosity became a platform
2. **Kids are the best QA** - They immediately notice when story continuity breaks
3. **Edge Functions have limits** - Railway saved Christmas 2023 when everything was timing out
4. **Database design matters** - Spent 3 months cleaning up redundant status fields
5. **Context windows are hard** - Most of the engineering is working around AI limitations

---

## Repository Deep Dives

### The Real Engineering Challenges
- [Story Template System](./docs/innovations/story-template-system.md) - **Why one AI call isn't enough**
- [Entity Extraction](./docs/innovations/story-evolution-system.md) - **Universal story memory in any language**
- [AI Provider Factory](./docs/architecture/ai-provider-factory.md) - **Database-driven routing that saved the platform**

### Technical Implementation
- [Microservices Evolution](./docs/architecture/microservices-evolution.md) - **Edge Functions → Railway migration**
- [SmartButton Pattern](./docs/code-samples/smart-button/README.md) - **UX pattern that eliminated toast chaos**

---

## Contact & Portfolio

This is my nerdy hobby project documenting 47 years of pattern obsession meeting modern AI.

- **Portfolio:** [aeberhard.ai](https://aeberhard.ai) - The professional version
- **LinkedIn:** [Patric Aeberhard](https://www.linkedin.com/in/patricaeberhard/)
- **Email:** patric@aeberhard.ai

---

*A platform built for the joy of solving interesting problems. No business plan, no exit strategy, just the satisfaction of making complex systems work reliably.*