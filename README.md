# City, Filtered

A [Claude Code](https://claude.com/product/claude-code) skill that builds a personal weekly
events digest for any city: it scans the event platforms people actually use where you
live, drops anything you're already registered for, filters what's left through your own
interests, and hands you back a small designed "issue" instead of a raw listing dump.

It started as a one-person, one-city tool (Barcelona) and got rebuilt here so anyone can
point it at their own city and their own interests.

## What you get

- **Featured** - up to 8 events, ranked, each with a specific reason tied to *your* stated
  interests, a conflict callout if it clashes with something already on your calendar, and
  a direct link to RSVP yourself.
- **Classifieds** - everything else that survived deduplication, dense and uncapped, tagged
  by category, nothing silently dropped for being low-priority.
- **Already on the books** - what you're already registered for this period, pulled from a
  connected calendar, so you see your committed load before anything new gets suggested.
- Published as a Claude Artifact (a shareable link) and, optionally, saved as a local HTML
  file - a small designed "issue" with its own identity, not a generic template.

**This skill never registers, pays, or submits anything on your behalf.** It discovers and
filters. Every pick is a link back to the original source - you decide, you click, you pay.

## Install

1. Clone this repo straight into your Claude Code skills directory:
   ```bash
   git clone https://github.com/vincenzodivincenzo/city-filtered ~/.claude/skills/city-filtered
   ```
   Your personal `config.yaml` is written into that same folder and is git-ignored, so a
   later `git pull` updates the skill without touching your setup.
2. In Claude Code, run:
   ```
   /city-filtered init
   ```
   This walks you through a short setup conversation: your city, which platforms are
   actually relevant there (it searches for this rather than assuming Western defaults),
   any personal/community source you belong to, calendar integration, and - the part that
   makes the output personal - what you actually care about.
3. After that, just run:
   ```
   /city-filtered
   ```
   any time you want a new issue. To get it weekly without asking, wire it into Claude
   Code's `schedule` skill using the cadence you set during init.

## Requirements

- Claude Code with a way to browse the web (an MCP browser connector, or the built-in
  Browser tool).
- Optional but recommended: a calendar connector (Google Calendar or similar), so the skill
  can tell what you're already registered for and flag real conflicts.
- Optional: a connector or URL for any coworking space, gym, or club that runs its own
  member events - these are usually the highest-signal source of all, since you're already
  committed to that space.

## How the personalization works

Setup produces a single `config.yaml` (see `config.example.yaml` for the full annotated
schema) covering:

- **Location** - drives which source URLs get built.
- **Platforms** - Luma, Meetup, and Eventbrite are near-universal defaults; setup also
  searches for what's actually popular in your specific city (Somo, Shotgun, DesignMyNight,
  Peatix, connpass, whatever's relevant) instead of assuming one country's defaults apply
  everywhere.
- **Interests** - free-text, in your own words, tagged primary/secondary. This is what
  actually drives Featured placement and the personalized "why this fits you" line on each
  card.
- **A social-exposure goal**, if you have one - e.g. deliberately weighting small recurring
  low-stakes formats over one-off mixers. Entirely optional.
- **Standing exclusions** - things you never want suggested again, even if they'd otherwise
  match an interest.
- **Recurring commitments** that don't live in any calendar (training blocks, a standing
  class) - soft conflicts, flagged but never used to veto a pick.

None of this is specific to any one city or profession - the schema is generic, and the
skill's setup flow is what adapts it to wherever you actually are.

## Design philosophy

Read `SKILL.md` for the full instruction set the skill follows. Two things worth calling out:

- **A wrong assumption beats nothing, but a question beats a wrong assumption.** Setup asks
  rather than guessing at your city's platforms or your own priorities; a run states
  plainly when a source failed or a dedup pass was incomplete, rather than producing a
  confidently-wrong "complete" issue.
- **The visual identity is designed fresh per city**, grounded in something real about that
  city rather than a reused template - the masthead concept (`"<City>, Filtered"`) stays
  consistent issue over issue once you've set it, but the actual design should feel like it
  belongs to your city, not to whichever city this was first built for.

## Contributing

This is a small skill, not a platform - issues and PRs adding coverage for platforms in
underrepresented regions, or fixes to the setup flow, are welcome. Forks that ship a
ready-made design for one city (palette, type, a motif drawn from that city's own visual
vernacular) are encouraged too; keep this repo the city-agnostic baseline.

## License

MIT - see `LICENSE`.
