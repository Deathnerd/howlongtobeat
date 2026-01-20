# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

PHP wrapper library for fetching game duration data from HowLongToBeat.com. Published as `askancy/howlongtobeat` on Packagist.

## Commands

```bash
# Install dependencies
composer install

# Run all tests
vendor/bin/phpunit tests

# Run a specific test file
vendor/bin/phpunit tests/SearchTest.php

# Validate composer configuration
composer validate
```

## Architecture

The library uses a two-phase extraction pattern:

**Entry Point:**
- `HowLongToBeat` - Main class providing `search($query, $page)` and `get($id)` methods. Handles dynamic API key extraction by scraping the website's JavaScript bundles for the API endpoint path.

**Extractor Pattern:**
- `Extractor` interface defines `extract(): array` contract
- `JSONExtractor` - Parses game data from API JSON responses (ID, title, image, summary times, statistics)
- `CrawlerExtractor` - Extracts platform-specific time tables from HTML using Symfony DomCrawler

**HTTP Layer:**
- `HttpClientCreator` - Factory for Guzzle client with required User-Agent and referer headers

**Utilities:**
- `Utilities` - Time/number formatting helpers (seconds to hours, K-abbreviations)

## Key Technical Details

- API key is extracted dynamically from `_app-*.js` bundle and cached statically
- The `search()` method retries with fresh API key on 404 errors
- The `get()` method extracts JSON from `#__NEXT_DATA__` script tag, then augments with DOM-scraped platform tables
- Tests are integration tests hitting the live HowLongToBeat API
- Uses PHPUnit 11 with `#[\PHPUnit\Framework\Attributes\Test]` attribute syntax

## Namespace

```php
use Askancy\HowLongToBeat\HowLongToBeat;
```
