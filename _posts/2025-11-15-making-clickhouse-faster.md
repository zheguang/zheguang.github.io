---
layout: post
title: "Making the fastest database faster -- My Contributions to ClickHouse 25.10"
categories: [blog, clickhouse]
tags: [clickhouse]
---
Three of my pull requests are merged into ClickHouse 25.10:
- 5x faster text search on prefix and suffix
- 30% speedup on case-insensitive affix search using SIMD
- Fix parallel data ingestion for constant common table expressions

[Content](https://github.com/zheguang/Notebook/blob/main/ClickHouse/Contributions-25-10/writeup.md)
