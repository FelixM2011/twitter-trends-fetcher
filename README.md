# Twitter Trends Fetcher — Hermes Skill

A Hermes agent skill that fetches real-time trending hashtags from [getdaytrends.com](https://getdaytrends.com). Provides country-specific filtering with headless-browser automation, no API keys required.

<!-- TOC -->
- [Twitter Trends Fetcher &ndash; Hermes Skill](#twitter-trends-fetcher--hermes-skill)
  - [Overview](#overview)
  - [Features](#features)
  - [How It Works](#how-it-works)
  - [Installation (Hermes Agent)](#installation--hermes-agent)
  - [Usage](#usage)
  - [Configuration](#configuration)
  - [Supported Countries and Timeframes](#supported-countries-and-timeframes)
  - [Pitfalls and Edge Cases](#pitfalls-and-edge-cases)
- [Project Structure](#project-structure)
- [License](#license)
<!-- TOC -->

## Overview

This skill powers an automated browser workflow: it navigates to a country-specific page on `getdaytrends.com`, parses the rendered table of trending topics, and returns ranked hashtags with their direct links. All scraping is done via headless or normal-browser invocation &mdash; no secret keys or third-party APIs are involved.

By default it returns up to 15 trends; you can change that in config if needed. The skill extracts position rank (as an integer), the trend text, and a clickable URL pointing back to Twitter/X's trend page for that hashtag.

## Features

- **No API keys required** &mdash; uses public-facing pages on getdaytrends.com
- **140+ supported countries/regions** via configurable URL slug
- **Time-frame control**: `4h`, `8h`, `12h` (default), `24h`, or `7d`.
- **Headless-ready** &mdash; can run server-side in a no-GUI headless-browser mode.

## How It Works

The agent skill uses Hermes' built-in `browser_navigate()` / `browser_snapshot()` tools to load the target URL, locate the second `<table>` on the page (index&nbsp;1) which lists trends with absolute positions, and extracts up to *N* rows past the header row into a structured list of trend objects. If no matching table is found it logs a warning instead of hallucinating results.

## Installation &ndash; Hermes Agent

The skill must live under your Hermes skill directory (`~/.hermes/skills/`). To install:

```bash
mkdir -p ~/.hermes/skills/media/twitter-trends-fetcher/references
cp SKILL.md            ~/.hermes/skills/media/twitter-trends-fetcher/
cp references/getdaytrends_extraction.md ~/.hermes/skills/media/twitter-trends-fetcher/references/
```

After that the Hermes agent will pick it up on its next run and make it available via `skill_view(name='twitter-trends-fetcher')`.

## Usage

The skill is invoked by the Hermes agent during a conversation turn. The agent calls `browser_navigate(url)` with the constructed country URL, then `browser_snapshot()` to read the rendered HTML table, and finally parses the rows into a plain-text list that gets returned to the user.

**Example interaction (conceptual):**

- User: &ldquo;Give me the Twitter Trends for Germany?&rdquo;
- Agent internally uses browser tools to navigate to `https://getdaytrends.com/germany?time=24h`, extracts the second table, and returns a ranked list.

## Configuration

No runtime config is needed beyond what the Hermes gateway exposes to the browser tool. Simply set the country code in the URL.

## Example for Supported Countries and Timeframes

| Country   | Slug       |
|-----------|------------|
| Germany   | germany    |
| Austria   | austria    |
| Switzerland | switzerland |
| United States | united-states |

The supported Country is in the english language. Example: greece, argentinia, new-zealand,...

Time frames: `4h`, `8h`, `12h` (default), `24h`, `7d`.

## Pitfalls and Edge Cases

- **Cloudflare 522 errors**: Sometimes getdaytrends.com blocks headless browsers. Retry or use a proxy.
- **Table truncation**: If the output is cut short, check that `browser_snapshot(full=true)` loads enough rows. Fallback: return an empty list and log a warning instead of hallucinating results.
- **Time-frame filters require URL parameter manipulation**. Not all time frames are available for every country.

---

*These notes are kept as a reference for future agents.*
