# Pinterest ROI Data Scraper

A production-ready Python web scraper for collecting Pinterest advertiser case studies and ROI data from public sources to validate investment theses.

## Overview

This scraper collects performance metrics and case studies from:
- Pinterest Business Blog (business.pinterest.com)
- Marketing agency blogs
- Public earnings transcripts and press releases
- Brand success stories

## Features

- **Respects robots.txt** - Checks and honors robots.txt rules for all domains
- **Rate limiting** - Implements 2-5 second delays between requests to avoid overwhelming servers
- **Error handling** - Comprehensive retry logic with exponential backoff
- **Data validation** - Cleans and validates extracted metrics
- **Pre-populated data** - Includes verified data from Pinterest earnings calls and press releases
- **Multiple export formats** - JSON, CSV, and TXT outputs

## Metrics Extracted

The scraper uses regex patterns to extract:
- Revenue increase percentages
- ROAS (Return on Ad Spend)
- ROI improvements
- Conversion rate lifts
- CPA (Cost Per Acquisition) reductions
- Traffic growth
- Purchase/checkout increases

## Installation

1. Clone or download this repository

2. Install dependencies:
```bash
pip install -r requirements.txt
```

## Usage

Run the scraper:
```bash
python pinterest_roi_scraper.py
```

The script will:
1. Load pre-populated data from public Pinterest sources
2. Attempt to scrape live case studies (respecting robots.txt)
3. Extract metrics using regex patterns
4. Validate and deduplicate data
5. Export to three formats

## Output Files

### 1. pinterest_roi_data.json
Full structured database containing all case studies with:
- Source information
- Extracted metrics
- Brand names
- Categories (Platform Performance, Brand Case Study, Retail Media Partnership, etc.)
- Additional metadata

Example structure:
```json
{
  "metadata": {
    "scrape_date": "2024-01-15T10:30:00",
    "total_case_studies": 15
  },
  "case_studies": [
    {
      "source": "Pinterest Q3 2024 Earnings Call",
      "title": "Performance+ Platform Metrics",
      "brand": "Performance+ (Aggregate)",
      "metrics": {
        "conversion_lift": [11.0],
        "roi": [57.0]
      }
    }
  ]
}
```

### 2. pinterest_roi_data.csv
Flattened metrics for Excel analysis with columns:
- source, title, brand, category
- metric_name_avg (average value)
- metric_name_max (maximum value)
- metric_name_count (number of data points)
- Additional metadata fields

### 3. pinterest_roi_summary.txt
Executive summary report including:
- Summary statistics by category
- Overall performance metrics (avg ROAS, ROI, conversion lift)
- Platform demographics and highlights
- Investment thesis validation points
- Data sources list

## Pre-Populated Data Included

The scraper includes verified data from public sources:

### Performance+ Platform Metrics
- 85% adoption among top 1,000 advertisers
- 11% CVR (conversion rate) lift
- 57% profit improvement

### Retail Media Partnerships
- Instacart (third-party ads integration)
- Kroger (shoppable integration)
- Amazon Ads (third-party platform)

### Brand Case Studies
- Häagen-Dazs (CPG/Food vertical)
- Pernod Ricard (Premium spirits)
- Luxury brand portfolio (Fashion, jewelry)

### Platform Demographics
- 498M monthly active users (Q3 2024)
- 42% Gen Z users
- 85% use Pinterest for shopping planning
- 45% high-income users (>$100k household income)
- 89% in discovery/planning mode

## Configuration

Edit the `PinterestROIScraper` class to customize:

### Rate Limiting
```python
self.min_delay = 2  # Minimum seconds between requests
self.max_delay = 5  # Maximum seconds between requests
```

### Request Headers
```python
self.session.headers.update({
    'User-Agent': 'Your User Agent',
    # ... other headers
})
```

### Regex Patterns
Add or modify patterns in `self.metric_patterns` dictionary to extract additional metrics.

## Best Practices

1. **Run during off-peak hours** - Minimize impact on target servers
2. **Monitor logs** - Check `pinterest_scraper.log` for errors or rate limit warnings
3. **Update pre-populated data** - Regularly update with latest earnings data
4. **Respect rate limits** - Increase delays if you encounter 429 errors
5. **Verify data** - Cross-reference extracted metrics with source material

## Logging

All operations are logged to:
- Console (INFO level and above)
- `pinterest_scraper.log` (detailed logs with timestamps)

Log levels:
- INFO: Normal operations and progress
- WARNING: Rate limits, blocked URLs, validation issues
- ERROR: Request failures, parsing errors
- DEBUG: Detailed debugging information

## Error Handling

The scraper handles:
- **Network errors** - Retry with exponential backoff (3 attempts)
- **Rate limits (429)** - Progressive delays (10s, 20s, 30s)
- **Blocked by robots.txt** - Skips URL and logs warning
- **Invalid data** - Filters out and logs validation errors
- **Duplicate content** - Deduplicates based on source + title

## Limitations

1. **Dynamic content** - Cannot scrape JavaScript-rendered content (no Selenium/Playwright)
2. **Authentication** - Cannot access content behind login walls
3. **Site structure changes** - Scrapers may need updates if sites redesign
4. **Rate limits** - Aggressive scraping may trigger temporary blocks

## Legal & Ethical Considerations

This scraper:
- ✅ Respects robots.txt directives
- ✅ Implements rate limiting
- ✅ Only accesses publicly available information
- ✅ Uses for legitimate investment research
- ✅ Includes proper User-Agent identification

**Important**:
- Review each website's Terms of Service before scraping
- Use data responsibly and in accordance with applicable laws
- Consider contacting site owners for API access to large-scale data needs

## Troubleshooting

### "Blocked by robots.txt"
- The site's robots.txt disallows scraping
- Solution: Use pre-populated data or contact site for API access

### "Rate limited" errors
- Increase `min_delay` and `max_delay` values
- Run during off-peak hours

### No case studies found
- Sites may have changed structure
- Check logs for fetch errors
- Verify URLs are still valid

### Invalid metric values
- Regex patterns may need adjustment
- Check `raw_text_sample` in JSON output to debug

## Investment Research Use

This tool is designed for:
- Validating advertising platform performance
- Quantifying ROI for platform investments
- Benchmarking advertiser results across verticals
- Analyzing platform adoption trends
- Comparing performance metrics

**Not suitable for**:
- Real-time trading decisions
- Sole basis for investment decisions
- Redistribution of proprietary data
- Commercial resale of collected data

## Contributing

To improve the scraper:
1. Add new regex patterns for additional metrics
2. Add new data sources (with robots.txt compliance)
3. Improve parsing logic for specific websites
4. Update pre-populated data with latest earnings
5. Enhance data validation rules

## License

This tool is provided for educational and research purposes. Ensure compliance with:
- Target websites' Terms of Service
- Applicable data protection regulations (GDPR, CCPA, etc.)
- Securities regulations regarding investment research

## Support

For issues or questions:
1. Check `pinterest_scraper.log` for error details
2. Verify dependencies are installed correctly
3. Ensure network connectivity
4. Review robots.txt for target domains

## Version History

**v1.0.0** - Initial release
- Pinterest Business Blog scraper
- Agency blog framework
- Pre-populated earnings data
- JSON/CSV/TXT exports
- Comprehensive error handling

## Future Enhancements

Potential improvements:
- [ ] Add Selenium for JavaScript-rendered content
- [ ] Implement proxy rotation for large-scale scraping
- [ ] Add sentiment analysis for case study text
- [ ] Create visualization dashboard for metrics
- [ ] Add scheduled/automated runs with cron
- [ ] Implement diff detection for tracking changes over time
- [ ] Add email alerts for new case studies
- [ ] Create comparison reports across quarters

## Disclaimer

This scraper collects publicly available information for research purposes. The author is not responsible for:
- Misuse of the tool
- Violations of website Terms of Service
- Investment decisions based on collected data
- Data accuracy or completeness

Always conduct thorough due diligence and consult with financial professionals before making investment decisions.
