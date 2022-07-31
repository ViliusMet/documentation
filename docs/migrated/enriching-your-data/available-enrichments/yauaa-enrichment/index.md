---
title: "YAUAA enrichment"
date: "2020-02-14"
sidebar_position: 30
---

YAUAA (Yet Another User Agent Analyzer) enrichment is a powerful user agent parser and analyzer.

It uses [YAUAA API](https://yauaa.basjes.nl/) to parse and analyze the user agent string of an HTTP request and extract as many relevant information as possible about the user's device and browser, like for instance the device class (Phone, Tablet, etc.).

<table class="has-fixed-layout"><tbody><tr><td class="has-text-align-center" data-align="center"><strong>YAUAA parsing relies entirely on in-memory <em>HashMap</em>s and require roughly 400 MB of RAM (see </strong><a href="https://yauaa.basjes.nl/README-MemoryUsage.html">here</a><strong>). Additional memory is also needed if caching is enabled (by default).</strong></td></tr></tbody></table>

There is no interaction with an external system.

## Configuration

- [Schema](https://github.com/snowplow/iglu-central/blob/master/schemas/com.snowplowanalytics.snowplow.enrichments/yauaa_enrichment_config/jsonschema/1-0-0)
- [Example](https://github.com/snowplow/enrich/blob/master/config/enrichments/yauaa_enrichment_config.json)

Only one parameter can be set in the configuration : `cacheSize`. This field determines the number of already parsed user agents that are kept in memory for faster processing. If set to 0, caching is disabled. If not set, a default size is used for the cache (10000).

## Input

This enrichment uses the field `useragent`.

## Output

This enrichment adds a new derived context to the enriched event with [this schema](https://github.com/snowplow/iglu-central/blob/master/schemas/nl.basjes/yauaa_context/jsonschema/1-0-1) (since enrich 1.4.0).

If a field can't be figured out by the algorithm, it won't be in the output. But some fields can have value _UNKNOWN_.

The only field that will always be present is `deviceClass`.

Here is an example of a derived context attached by this enrichment for a page visited with a Samsung Galaxy S9:

```
{
  "schema":"iglu:com.snowplowanalytics.snowplow/yauaa_context/jsonschema/1-0-1",
    "data": {
        "deviceClass":"Phone",
        "deviceName":"Samsung SM-G960F",
        "deviceBrand":"Samsung",
        "operatingSystemClass":"Mobile",
        "operatingSystemName":"Android",
        "operatingSystemVersion":"8.0.0",
        "operatingSystemNameVersion":"Android 8.0.0",
        "operatingSystemVersionBuild":"R16NW",
        "layoutEngineClass":"Browser",
        "layoutEngineName":"Blink",
        "layoutEngineVersion":"62.0",
        "layoutEngineVersionMajor":"62",
        "layoutEngineNameVersion":"Blink 62.0",
        "layoutEngineNameVersionMajor":"Blink 62",
        "agentClass":"Browser",
        "agentName":"Chrome",
        "agentVersion":"62.0.3202.84",
        "agentVersionMajor":"62",
        "agentNameVersion":"Chrome 62.0.3202.84",
        "agentNameVersionMajor":"Chrome 62"
   }
}
```

The full output possiblities generated by the YAUAA algorithm can be found [here](https://yauaa.basjes.nl/README-Output.html).