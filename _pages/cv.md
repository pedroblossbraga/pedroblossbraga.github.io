---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Education
======
* M.S. in M.Sc. Computational and Applied Mathematics (CAM) - Friedrich-Alexander Universität (FAU), Erlangen, Germany, 2025 (expected)
  * Specialization in Optimization and Numerical Analysis
* B.S. in B.ASc. Applied and Computational Mathematics - University of São Paulo (USP), São Paulo, Brazil, 2022
  * Thesis: "Support Vector Machines: Classification, Kernels, Regression, and Optimization"
  * Thesis Abstract: Study variants of the Support Vector Machine (SVM) algorithm, deriving the classification formulations C-SVC and ν-SVC, the regression formulations ε-SVR and ν-SVR, in their linear or kernelized forms, with convex optimization techniques, such as the Lagrangian, to build the dual optimization form; kernel properties, reproducing kernel maps and Hilbert spaces; and implementation of examples in Python.
  * Thesis Advisor: Nina S. T. Hirata, PhD.
  * Thesis Grade: Full remarks.
  * Minor: Statistical Economics / Econometrics.
* Supervised Studies / Undergraduate Scientific Initiation: Topological Brouwer Degree Theory. Academic Advisor: Pierluigi Benevieri, PhD.
* Supervised Studies / Undergraduate Scientific Initiation: Image Classification and Segmentation. Academic Advisor: Nina S. T. Hirata, PhD.

Work experience
======
* Metatimbre AI: Partner
  * Leading  AI solutions, mainly with Natural Language Processing (NLP) and Computer Vision, with applications in pattern detection in voice, traffic safety, and others.
  * We’ve built the first AI in Brazil that detects and quantifies alcohol consumption through speech, with 82.9% accuracy, using hundreds of parameters.

* Window Finance: Data Scientist
  * Implemented trading strategies and backtests with traditional and novel models.
  * Developed AI pipelines to identify investment opportunities and predict financial indicators.
  * Collaborated on building decentralized finance platforms.
  * Tech Stack: Python, Prefect, SQL, TensorFlow, scikit-learn, Google Cloud, Docker.

* Sami Healthcare: Data Scientist, Sr
  * Improved customer acceptance rates by 2.8x with an ensemble model for imbalanced classification.
  * Built Deep Learning models for time series forecasting to enhance acquisition predictions.
  * Tech Stack: Python, SQL, Databricks, AWS, TensorFlow, imblearn.

* Albert Einstein Hospital: Data Scientist
  * Implemented an AI model for predicting absenteeism due to mental health issues using XGBoost.
  * Tech Stack: R, SQL, Docker, PostgreSQL.
 
* Safra Bank: Money Laundering Prevention Analyst
  * Developed systems for fraud detection and suspicious activity alerts.
  * Tech Stack: Python, R, SQL, SAS.
    
* Netpartners / Weduu:  Data Scientist, Trainee to Jr
  * Implemented a Sales forecasting model for large retailers.
  * Tech Stack: Python, R, SQL, BigQuery.

Technical Skills
======
* Programming: Python, R, SQL, Bash/Shell, C/C++
* Libraries/Packages: Scikit-learn, Tensorflow, Keras, XGBoost, pandas, scipy, etc
* Cloud tools:  Google Cloud, AWS, Docker, Prefect

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
Talks
======
  <ul>{% for post in site.talks reversed %}
    {% include archive-single-talk-cv.html  %}
  {% endfor %}</ul>
  
Teaching
======
  <ul>{% for post in site.teaching reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
