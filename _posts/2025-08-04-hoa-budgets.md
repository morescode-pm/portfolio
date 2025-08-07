---
layout: post
tags: [sql, googlebigquery, tableau]
title: HOA Budgeting - Extracting clarity from a PDF
published: true
---

Do you live in an HOA and get a once-per-year pdf with the breakdown of where your dues were allocated? If yes, then this post is for you!
Instead of trying (and failing) to understand what on earth the figures in a poorly structured table mean - let's build a dashboard in Tableau.

Here's the [Dashboard][1] - embedded up front so you can get right into the digging:
<div class="full-width">
    <div id="vizWrapper">
        <tableau-viz
            id="tableauViz"
            src="https://public.tableau.com/views/HOABudgetReview/HOABudgetBreakdown"
            toolbar="bottom"
            hide-tabs
            device="desktop">
        </tableau-viz>
    </div>
</div>


Keep reading to see my approach and how you could do this, too.

## Introduction
My dues keep going up. Sound familiar? Wondering why? Me too.  



Starts with a pdf table nobody wants to read.
Get a notification that your dues are going up 15%

Spend some time trying to understand where the money is going.


1. Transcribe the pdf to a google sheet (could use a pdf reader tool, could use OCR, could use AI)
2. Create a table in BQ that links to that google sheet (and updates when the sheet is updated)
3. Create a view that unpivots or unions columns
4. Import into Tableau - either with a connected sheet or tableau desktop free google bigquery connector + extract
5. Note that changes to the source google sheet are reflected in the transformed google sheet on a refresh.



[1]: https://public.tableau.com/app/profile/paul.moresco/viz/HOABudgetReview/HOABudgetBreakdown