---
layout: post
tags: [SQL, GoogleBigQuery, Tableau]
title: HOA Budgeting - Extracting clarity from a PDF
published: false
---

Starts with a pdf table nobody wants to read.
Get a notification that your dues are going up 15%

Spend some time trying to understand where the money is going.


1. Transcribe the pdf to a google sheet (could use a pdf reader tool, could use OCR, could use AI)
2. Create a table in BQ that links to that google sheet (and updates when the sheet is updated)
3. Create a view that unpivots or unions columns
4. Import into Tableau - either with a connected sheet or tableau desktop free google bigquery connector + extract