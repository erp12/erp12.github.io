---
title: The Revival of PyshGP
date: 2019-02-24
draft: false
---

I have revived [pyshgp](#). A python package for doing [PushGP](https://erp12.github.io/push-redux/).

Program synthesis, or "programming by example," has been steadily getting more attention each year. Some simple systems are relatively well known, such as [Excel's Flash Fill](#) and [SKETCH](#). The more capable (and complex) systems have been largely relegated to niche research communities, each busy exploring their respective regions of the frontier. As the field continues to gain traction, we would hope to see a convergence of ideas and the emergence of well understood techniques and tools. One indication of how far the field of program synthesis has progressed to look at the applications of the "state-of-the-art".

Consider the "[Genetic Improvement](http://geneticimprovementofsoftware.com/)" workshop at the GECCO conference. The workshop is dedicated to research surrounding the use of [Genetic Programming](#) to improve pre-existing software. The workshop is relatively young, starting in 2015, however the [upcoming 2019 installment](https://gecco-2019.sigevo.org/index.html/Workshops#id_Genetic%20Improvement%20(GI)) is already choosing to focus on industry applications.

PushGP is a program synthesis technique that has been around since 2001, and is still actively researched. Throughout its life it has been developed enough to accomplish things most other program synthesis systems are incapable of. The programs produced by PushGP can utilize all of the standard data types and controls structures, as well as some simple data structures. PushGP is easily extendable to specific use cases and has seen impressive human-competitive coding results. For example, PushGP was used in the [discovery of novel quantum computer programs](http://faculty.hampshire.edu/lspector/aqcp/) previously unknown to humanity, and has achieved human competitive results in [finding algebraic terms in the study of finite algebras](http://www.cs.bham.ac.uk/~wbl/biblio/gecco2008/docs/p1291.pdf).

The evidence of PushGP's capabilities have resulted some interesting papers and discussions at conferences, but the communities involved are rather small and the real-world applications lean strongly academic.

The **newly revived** [PyshGP](https://github.com/erp12/pyshgp) project will be solely focused on maintaining a well understood PushGP implementation that is easy to use and application focused. Through this project, PushGP will hopefully reach a larger audience across a wider breadth of the field of program synthesis.

## The history of PyshGP and what went wrong.

I started pyshgp when I was student. I used it for a handful of course projects, and I found myself wanting "PushGP in Python" frequently enough that I felt I should attempt to engineer a long-lived solution. I didn't have the skills or experience in software design to do it well, but I tried anyways. I was further encouraged after I started attending the annual GECCO conferences and frequently met individuals eager to try PushGP but unable to work with current implementations due to technical issues.

The most advanced and bleeding-edge PushGP implementation is [Clojush](#). It is written in [Clojure](#). I love Clojure. I love Clojure so much that my coworkers have started muttering "wait for it" or "here it comes" right before I say my catchphrase: "This would be so much simpler if we were using Clojure."

Unfortunately, Clojure is not good for everything. It is virtually unheard of in the AI/ML field because it lacks the ecosystem of tools needed for non-engineers to perform their day-to-day tasks of modeling, visualization, and exploratory data analysis. Compare this to Python which is the host language of numpy, sci-kit learn, tensorflow, keras, seaborn, etc.

If PushGP is going to widen its audience, it would make sense to have a Python implementation.

Unfortunately, PyshGP failed to attract many users and was likely never used for any significant applications outside of my personal research projects. Before returning to the project, I thought it would be important to reflect on why this happened.

### Lost in translation.

The first mistake was a direct consequence of my previously mentioned lack of experience. The first version of PyshGP was nearly a direct translation of Clojush into Python.

Clojush is written purely functionally with immutable data structures. Python has a lot of mutability and is primarily an object oriented language.

Clojush is a platform to support quick iterations of active research projects. The users of Clojush are not held back by the accumulation of technical debt because their main goal is the answer a research question, test a hypothesis, or gather data for publication. Clojush solves these needs, and the initial version of PyshGP was mostly redundant.

From day 1, PyshGP suffered from an identity crisis, both in terms of who the intended users were and which paradigms were used in the source code. This heavily contributed to PyshGP's abandonment.

### My project. My use case.

Once the initial version of PyshGP was working, I wanted to use it.

I was primarily engaging with PushGP through contributions to academic research projects. I started to use PyshGP as the platform I used to run my experiments. This resulted in dozens of branches and GitHub issues, none of which were justifiable contributions to the main project.

I should have separated the open source project of PyshGP from my own personal applications.

Eventually the project bloated to the point where maintenance became painful. The codebase contained multiple superfluous features as well as dead code.

The few people that showed interest expressed they were having difficulty with certain aspects of the project. Synthesized programs could not be saved and reused. The naive parallelism was unhelpful at best. Things were getting worse, not better.

> [Neal Page: He says we’re going the wrong way…](https://www.youtube.com/watch?v=_akwHYMdbsM)
>
> [Del Griffith: Oh, he’s drunk. How would he know where we’re going?](https://www.youtube.com/watch?v=_akwHYMdbsM)
>
> -- Neal and Del, **Planes, Trains & Automobiles**

I knew it would take a lot of work to fix things, and at the time I was still relatively inexperienced. Without the skills to do better, I let the project sit.

### The revival.

I returned to the project a better engineer than when I first started PyshGP. This time the motivation was clear: **Provide the wider AI/ML community with a stable, usable, implementation of PushGP that is suitable for application oriented use cases**.

The PyshGP codebase has been completely rewritten from scratch. The abstraction has been designed so that users do not need an understanding of the internals to use the package.

PyshGP will not accept contributions that do not have a clear and demonstrable purpose. This includes contributions about open research questions that have not achieved a positive result yet. Any PushGP research done with PyshGP can be done on a fork or local branch without polluting the main repository.

PyshGP will limit its scope to providing PushGP as a library. There will be no CLI. There will be no functionally starting new benchmark runs. Utilities can (and will) be created in separate applications for these purposes.

### What comes next?

Development of PyshGP will continue at a slow and methodical pace. Contributions are always welcome via [Issues](https://github.com/erp12/pyshgp/issues) and Pull Requests on GitHub.

Auxiliary utilities for benchmarking PyshGP will be created in separate repositories.

I will be writing individual posts on narrow parts of the PyshGP abstraction that will be insightful to users and a good jumping off point for future contributors. Topics include:

  - PushEstimator vs SearchAlgorithms
  - Genomes Vs Code Blocks/Programs
  - Parent Selection
  - Variation
  - User Defined Instructions

I would be thrilled to hear about any application of PyshGP. It would be very helpful do a case study of various applications to determine where the tool needs the most improvement.
