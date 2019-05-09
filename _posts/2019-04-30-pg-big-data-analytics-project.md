---
layout: single
title:  "Postgraduate Studies in Big Data & Analytics in Business and Management"
date:   2019-04-30 18:34:12 +0100
excerpt: "Today I presented my project on car crashes, the conclusion of quite the journey"
categories: [tech]
tags: [Python, Big Data]
---
A year ago, In enrolled in the "Postgraduate Studies in Big Data & Analytics in Business and Management" at [KU Leuven](https://feb.kuleuven.be/permanente-vorming/bigdataanalytics). And today, the project I've been working on for the past 3 months came to a conclusion with the final presentation.

The dataset I've been working on was collected by The Belgian federal police  during 2014-2016 and contains over 52K car crashes. It has been enriched by Informatie Vlaanderen with additional information on weather conditions, road conditions and severity of the accident.

 In this project I went through all stages of data cleaning, and I must admit: all rumours you have heard, are true. Data is perfect and complete at all, and it even makes no sense at all (Speed limit of 999 km/h, seriously?). Data cleaning does indeed take 80% of your available time.

 I've used random forest (the ensemble variant of decision tree) to predict the severity of the car crash. The performance of this model was not at all accurate, because of the huge imbalance of the severity: over 95 percent of the car crashes only had "gewonden" (casualties). This is just great from a humanity point of view, not so much from a data perspective.

 So to tackle this issue, I've been over- and undersampling the data. The accuracy improved a bit, but still a lot of room for improvement.

 After all, I enjoyed the program (even during the late evening classes) and although challenging, the project was fun to work on! The report itself is available on [github](https://github.com/pieterjd/pg-dissertation/blob/master/report/report.pdf).
