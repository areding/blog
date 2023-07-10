+++
title = "ISYE 6420: Bayesian Statistics Course Update"
description = "Redoing an older Bayesian Statistics course with more modern tools"
date = 2022-01-11
draft = false
tags = ["Bayesian", "probabilistic programming", "pymc", "python", "statistics"]
image = "/images/bugstopymc/bugspymcthumb.jpg"
summary = "I created this site to address the most common student complaints and questions for the Bayesian Statistics course in OMSA and OMSCS. I redid the lecture examples in Python and PyMC and added notes that provide context to the lectures and address the most common questions we get about each one."
[listGallery]
images = [
  {src = "/images/bugstopymc/btp_ss6.jpg", alt = "Screenshot from project website"},
  {src = "/images/bugstopymc/btp_ss4.jpg", alt = "Screenshot from project website"},
  {src = "/images/bugstopymc/btp_ss1.jpg", alt = "Screenshot from project website"},
]
+++

## Redoing an older Bayesian statistics course with more modern tools

{{< gallery >}}
- /images/bugstopymc/btp_ss6.jpg
  Screenshot from project website
- /images/bugstopymc/btp_ss4.jpg
  Screenshot from project website
- /images/bugstopymc/btp_ss1.jpg
  Screenshot from project website
  {{</ gallery >}}


During my second semester as a TA, I created this site to address the most common student complaints and questions. At the time, the most frequent source of [dissatisfaction](https://www.omscentral.com/courses/introduction-to-theory-and-practice-of-bayesian-statistics/reviews) was the course’s use of older software, namely [BUGS](https://www.mrc-bsu.cam.ac.uk/software/bugs/). So I redid the lecture examples in Python and PyMC.

A close second was the lecture quality—students found some of the lectures unapproachable for various reasons. To help with this, I’ve added notes that provide context to the lectures and address the most common questions we get about each one. As someone who originally took the class a little light on the necessary math and statistics knowledge, I hope my perspective helps other people coming in with similar backgrounds.

The project can be found [here](https://areding.github.io/6420-pymc/). The website was created with [Jupyter Book](https://jupyterbook.org/), and the examples make use of [PyMC](https://github.com/pymc-devs/pymc), [SciPy](https://docs.scipy.org/doc/scipy/index.html), [NumPy](https://numpy.org/), [ArViz](https://arviz-devs.github.io/arviz/), and [Pgmpy](https://pgmpy.org/).

### Current progress (last updated July 2023)

Over the summer, I'll be adding notes for each lecture, further explanation for existing examples, and new supplementary examples. As of July 10th, I'm finished with Unit 4. Lots of content for Unit 5 has already been written, hoping to finish incorporating that within a week.