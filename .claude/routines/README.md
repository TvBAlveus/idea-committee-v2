# Routines in diesem Repo

## idea-kill-committee
Tägliches Idea-Mining mit Kill-Committee-Methodik. Aktualisiert `index.html` (das auch über GitHub Pages live ist).

**Setup in Claude Routines (claude.ai/code/routines):**
1. New Routine → Name: "idea-kill-committee"
2. Add Repository: `TvBAlveus/idea-committee-v2` (Read+Write)
3. MCPs aktivieren: Tavily (für tavily_search/tavily_extract)
4. Schedule: Daily 07:00 (Cron: `0 7 * * *`) oder 2x täglich `0 7,19 * * *`
5. Prompt: Inhalt von `.claude/routines/idea-kill-committee.md` paste-en
