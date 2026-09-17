# Access SIDO MEET Skill

Agent-facing Skill for searching, reading, summarizing, and citing public content from [SIDO MEET](https://sido-meet.github.io/).

## Use

Clone this repository or copy it into your agent's Skill directory:

```bash
git clone git@github.com:sido-meet/access-sido-meet-skill.git
```

Then ask:

```text
Use $access-sido-meet to search SIDO MEET and answer with cited site content.
```

Agents without local Skill support can read the public guide at:

```text
https://sido-meet.github.io/skill/
```

## Data Sources

- Search index: `https://sido-meet.github.io/search.json`
- Popular index: `https://sido-meet.github.io/popular.json`
- Sitemap: `https://sido-meet.github.io/sitemap.xml`
- Article feed: `https://sido-meet.github.io/atom.xml`
- Notes feed: `https://sido-meet.github.io/notes/atom.xml`
- AI column feed: `https://sido-meet.github.io/ai/atom.xml`
