---
name: access-sido-meet
description: Search, read, summarize, and cite public SIDO MEET website content for questions about SIDO, AI engineering notes, LLM/Agent practice, Text-to-SQL, projects, bookmarks, thoughts, glossary terms, and site navigation. Use when an agent needs to answer from https://sido-meet.github.io or discover relevant SIDO MEET pages before responding.
---

# Access SIDO MEET

Use the public site indexes first, then fetch specific pages only when a result needs confirmation or more context. Prefer concise, cited answers grounded in SIDO MEET content.

## Public entry points

- Site root: `https://sido-meet.github.io/`
- Skill page: `https://sido-meet.github.io/skill/`
- Search index: `https://sido-meet.github.io/search.json`
- Popular index: `https://sido-meet.github.io/popular.json`
- Sitemap: `https://sido-meet.github.io/sitemap.xml`
- Robots policy: `https://sido-meet.github.io/robots.txt`
- Article feed: `https://sido-meet.github.io/atom.xml`
- Notes feed: `https://sido-meet.github.io/notes/atom.xml`
- AI column feed: `https://sido-meet.github.io/ai/atom.xml`

## Search workflow

1. Fetch `search.json` for broad discovery.
2. Match the user's terms against `title`, `summary`, `content`, `tags`, `categories`, `content_type_label`, and `subtype`.
3. Prefer records whose `content_type` matches the request:

| Request | Prefer |
|---|---|
| Long-form technical writing, tutorials, retrospectives | `article` |
| Reading, learning, experiments, project logs | `note` |
| Model-authored or disclosed AI writing | `ai` |
| External resources and saved links | `bookmark` |
| Short original observations | `thought` |
| Reusable concepts and definitions | `term` |
| Things SIDO built or maintains | `project` |

4. Open the top relevant `url` values on the site when the search record is not enough.
5. Cite the canonical page URL. For bookmarks and projects, include `external_url` only as a secondary source when it is relevant.

## Answering rules

- Answer in the user's language when clear; otherwise use Chinese for SIDO MEET questions.
- Ground claims in fetched SIDO MEET records or pages.
- Include links to the SIDO MEET pages used.
- State when no matching site content was found instead of inventing details.
- Distinguish SIDO's own writing from disclosed AI column content when `content_type` is `ai`.
- Keep summaries proportional: use search index summaries for quick questions; fetch full pages for detailed explanations, quotes, or recommendations.

## Useful direct pages

- Blogs: `https://sido-meet.github.io/blogs/`
- Notes: `https://sido-meet.github.io/notes/`
- AI column: `https://sido-meet.github.io/ai/`
- Projects: `https://sido-meet.github.io/projects/`
- Popular content: `https://sido-meet.github.io/popular/`
- Thoughts: `https://sido-meet.github.io/thoughts/`
- Glossary terms: `https://sido-meet.github.io/terms/`
- About: `https://sido-meet.github.io/about/`

## Minimal implementation example

```js
const index = await fetch('https://sido-meet.github.io/search.json').then((r) => r.json());
const query = 'agent memory';
const hits = index
  .map((item) => ({
    item,
    text: [
      item.title,
      item.summary,
      item.content,
      ...(item.tags || []),
      ...(item.categories || [])
    ].join(' ').toLowerCase()
  }))
  .filter(({ text }) => query.toLowerCase().split(/\s+/).every((term) => text.includes(term)))
  .slice(0, 5)
  .map(({ item }) => ({
    title: item.title,
    type: item.content_type,
    url: new URL(item.url, 'https://sido-meet.github.io/').href,
    summary: item.summary
  }));
```
