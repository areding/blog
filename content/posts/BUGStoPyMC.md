+++
title = "BUGS to PyMC"
description = "Redoing an older Bayesian Statistics course with more modern tools"
date = 2022-01-11
draft = false
tags = ["project", "Bayes", "probabilisticprogramming", "pymc"]

+++

## Redoing an older Bayesian Statistics course with more modern tools

I'm working on translating the examples from Professor Vidakovic's Bayesian Statistics course at Georgia Tech from MATLAB and OpenBUGS to Python (using libraries PyMC3 and pgmpy). As a former student and current TA, by far the number one complaint I've heard about the course is that OpenBUGS is outdated and a waste of time to learn. I chose PyMC because among the newer [Probabilistic Programming Languages](https://en.wikipedia.org/wiki/Probabilistic_programming#List_of_probabilistic_programming_languages) it seems the easiest to learn, especially since I'm most familiar with Python.

The project can be found [here](https://areding.github.io/6420-pymc/). It's made with [Jupyter Book](https://jupyterbook.org/), and makes use of PyMC 3, [PyMC 4](https://github.com/pymc-devs/pymc), [NumPy](https://numpy.org/), [Arviz](https://arviz-devs.github.io/arviz/), and [pgmpy](https://pgmpy.org/).

## Todo:

- [x] Mad scramble to get enough examples relating to each homework in the Spring 2022 semester.
- [ ] Finish adding all examples from the original course.
- [ ] Convert the remaining PyMC 3 examples to PyMC 4.
- [ ] Reorganize the table of contents and add tags for easier navigation.
- [ ] Rename each example to be a little more descriptive.
- [ ] Fix the look of the site:
  - [ ] Resize markdown tables.
  - [ ] Dark mode option?
  - [ ] Make syntax highlighting better-looking.
- [ ] Add more content relating to MCMC algorithms. Especially since the course only covers Metropolis and Gibbs but PyMC uses Hamiltonian/NUTS.
- [ ] Add FAQ.
- [ ] Add a centralized references page and/or more official references to each page (right now it's just in-line links).
- [ ] Improve visualizations.
- [ ] More content relating to prior elicitation.
- [ ] More content relating to model validation.

