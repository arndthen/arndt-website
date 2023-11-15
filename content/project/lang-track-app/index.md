---
author: Henriette Arndt
categories:
date: "2023-01-01"
draft: false
excerpt: An open source mobile tool to help researchers collect richer and more reliable survey data.
layout: single
links:
- icon: github
  icon_pack: fab
  name: app source code
  url: https://github.com/HumlabLu/HumlabLu
- icon: newspaper
  icon_pack: fas
  name: Paper 1
  url: https://journals.sagepub.com/doi/10.1177/02676583211020055
- icon: newspaper
  icon_pack: fas
  name: Paper 2
  url: https://onlinelibrary.wiley.com/doi/10.1111/lang.12555
subtitle: "Collaborators at Lund University: Marianne Gullberg, Jonas Granfeldt, Stephan Björck, Josef Granqvist"
tags:
title: LANG-TRACK-APP
---

## Objective
Researchers and educators are increasingly interested in understanding the role that different languages play in our everyday lives, with the aim of harnessing learner's perspectives to improve language education for all ‘from the bottom up’. This poses a methodological challenge, as existing research instruments are not well suited collecting data on language use outside of educational or experimental settings. Therefore we developed the [LANG-TRACK-APP](https://github.com/HumlabLu/HumLabLu), an open-source set of tools to help researchers collect richer and more reliable qualitative and quantitative data.

--- 

## Methods
We developed the LANG-TRACK-APP in a cross-functional team consisting of software and mobile developers, as well as researchers in linguistics and psychology. We collaborated on the design and implementation of three tools:

1. A mobile survey application (native on iOS and Android) for signalling research participants via push notifications and collecting survey responses;  
2. An administrative website for researchers to upload surveys, schedule notifications, and export data;  
3. A web service and database which serve as backend to the app and website, and store user lists, survey schedules, and questionnaire data.  

We started by conducting a scoping review of available tools and methods to uncover relevant barriers and opportunities for better meeting researchers' goals and requirements. The most important insight at this stage was that we needed to allow customised adaptations of the application to be able to meet the aims and needs of a wide range of potential research projects, and to give researchers complete control over the data flow and data management to comply with ethical guidelines and GDPR regulations.

Following an agile development process, we built an initial set of tools (MVP) which we consequently expanded and improved through iterative testing with researchers and research participants. We conducted several shorter pilot tests as well as two multi-year proof-of-concept studies to demonstrate usability of the LANG-TRACK-APP in research with two contrasting groups (university students and adult refugees) and to develop best practices for the integration of our new tools in the end-to-end research process (from study design to data analysis and communication of findings). 

---

## Outcomes

The full open source code, documentation, and conditions of use for the LANG-TRACK-APP tools are available for on [GitHub](https://github.com/HumlabLu/HumLabLu) for use under the Apache License, Version 2.0. We encourage all researchers to use and adapt these tools in the interest of Open Science, and require them to share any new iterations in order to secure the long-term sustainability of the LANG-TRACK-APP through continuous integration.

We are introducing the LANG-TRACK-APP to the research community through presentations at various international conferences and professional publications, which demonstrate the value of the app through the discussion of possible use cases and relevant research processes.

Furthermore, we are working to raise funds to develop a new version, the LANG-TRACK-APP-Vis, which will incorporate design principles of Persuasive Technology to visualize and provide feedback on learners’ everyday language use.
