---
layout: post
tags: [computervision, ux, ui, node, vercel, mongodb, jules]
title: Filtering 90% of blank wildife photos for a better UX
published: false
---

Our community wildlife classification/identification project is live at [rangers.urbanrivers.org][1].  
Using AI, we are massively fixing the boring part.  

# Introduction
Over the first 9 months, volunteers have labeled just over 60k images in our database (oct'24-jun'25).  
Of these, ~46k were labeled blank (grass/leaves/wind sets off the sensor) and ~14k were labeled as containing at least one animal.  

A human labeling rate of about 6.7k images per month with 77% of those being blanks.  

Extrapolating that number to the 200k+ images currently needing labels and blanks being 77-84%.. we could be looking at 154-168k blank images (😴) that should take another 21-25 months (~2 years) to complete. 

This post will describe how we're making use of the speciesnet (and megadetector) results from the first pass through our dataset.

# Project Steps so far:
1. [Generate Speciesnet Inference on dev and then production images][2]
2. [Try to fine-tune an imagenet model for a site specific classification system with limited data][3]
  - Explore results, classifications and confusion matrices
3. Process speciesnet inferences and append results to MongoDB records
  - [Compare Humans vs AI in Tableau][4]
  - **(This post)** - Implement aiResults filters for less human labeling fatigue.
  
  


Users have to identify animals - but most images are blank

As is 90 % of our images are blanks.


We ran speciesnet on Kaggle
We processed the output file to extract detections.

We built filters to show only images with at least 75% chance of an animal first.



Next Steps:
- 4. Reinforcment model training - Using complementary approaches for curating and fine tuning models

Using speciesnet inference to source images (done)
Using early imagenet/pytorch classification to source images (todo)

Confirming sourced images and repetitive rounds of cleaning + fine tuning  (todo)




[1]: https://rangers.urbanrivers.org/cameratrap
[2]: {% post_url 2025-05-22-camera-trap-computer-vision %}
[3]: {% post_url 2025-06-20-deploying-computer-vision %}
[4]: {% post_url 2025-06-24-exploring-ai-vs-human-cameratraps %}

