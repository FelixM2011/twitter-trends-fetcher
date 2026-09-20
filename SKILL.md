---
name: twitter-trends-fetcher
description: Fetch Twitter/X trends from getdaytrends.com with country-specific filters
category: media
---

## Skill Content
This skill fetches Twitter/X trends from getdaytrends.com with country-specific filters. It uses browser_navigate to access the target URL, extracts trending hashtags using browser_snapshot, and filters results by time frame via URL parameters.

### Steps:
1. Use browser_navigate to access https://getdaytrends.com/[country]
2. Extract trending hashtags from browser_snapshot
3. Filter by time frame via URL parameters (e.g., ?time=24h)
4. Return top 15 trends with URLs

### Pitfalls:
- Web_extract fails on getdaytrends.com - use browser tool instead
- Time frame filters require URL parameter manipulation
- Language-specific trends require country code in URL

### Verification:
- Confirm trends are from correct time frame
- Validate URLs are clickable and lead to trend details
- Ensure output format matches user preference (list of URLs only)
