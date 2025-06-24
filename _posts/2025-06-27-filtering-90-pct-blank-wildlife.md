---
layout: post
tags: [computervision, ux, ui, node, vercel, mongodb, jules]
title: Filtering 90% of blank wildife photos for a better UX
---

Our community wildlife classification/identification project is live at [rangers.urbanrivers.org][2].  
Using AI, we are massively fixing the boring part.  

Over the first 9 months, volunteers have labeled just over 60k images in our database (oct'24-jun'25).  
Of these, ~46k were labeled blank and ~14k were labeled as containing at least one animal.  
A human labeling rate of about 6.7k images per month with 77% of those being blanks.  

Extrapolating that number to the 200k+ images currently needing labels.. we could be looking at 154k blank images (😴). 
At our labeling rate - ignoring exhausion - that should take another 21 months (almost 2 years) to complete. 
Meanwhile we are still generating thousands of additional images.

This post will describe how we're making use of the speciesnet (and megadetector) results from the first pass through our dataset

Project Steps so far:
- 1. Generate Speciesnet Inference on dev and then production images
    - Explore results, classifications and detections
- 2. Try to fine-tune an imagenet model for a site specific classification system with limited data
    - Explore results, classifications and confusion matrices
- 3. Process speciesnet inferences and append results to MongoDB records
    - Compare Humans vs AI in Tableau
    - **(This post)** - Implement aiResults filters for less human labeling fatigue.




Users have to identify animals - but most images are blank

Blank motion capture images happen when grass/leaves/wind sets off the sensor.

As is 90 % of our images are blanks.


We ran speciesnet on Kaggle
We processed the output file to extract detections.

We built filters to show only images with at least 75% chance of an animal first.



Next Steps:
- 4. Reinforcment model training - Using complementary approaches for curating and fine tuning models

Using speciesnet inference to source images (done)
Using early imagenet/pytorch classification to source images (todo)

Confirming sourced images and repetitive rounds of cleaning + fine tuning  (todo)


[3]: {% post_url 2025-05-22-camera-trap-computer-vision %}

SpeciesNet was described by Gadot et al.[^1]

[^1]: Gadot, T., Istrate, Ș., Kim, H., Morris, D., Beery, S., Birch, T., & Ahumada, J. (2024). *To crop or not to crop: Comparing whole-image and cropped classification on a large dataset of camera trap images.* IET Computer Vision. Wiley Online Library.

[2]: https://rangers.urbanrivers.org/cameratrap