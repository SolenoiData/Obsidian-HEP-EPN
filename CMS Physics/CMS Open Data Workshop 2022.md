To access Open Data we use Docker, and specifically there are three images that are necessary for us to get familiar with.

·      Root Container
·      Python Container
·      CMSSW: Is the software that lets us extract the information from the original data

[Data Scouting](https://cms-opendata-workshop.github.io/workshop2022-lesson-dataset-scouting/) pre-lesson is how to access the data from the Open Data portal. 

[[ROOT]] is a framework in C++ to analyze data

[[CMSSW]] is the software used to access the objects you could find in the raw data that comes out of the detector.

## POET (Physics Object Extractor Tool)

[GitHub repository]([https://github.com/cms-opendata-analyses/PhysObjectExtractorTool/tree/odws2022-ttbaljets-prod/PhysObjectExtractor](https://github.com/cms-opendata-analyses/PhysObjectExtractorTool/tree/odws2022-ttbaljets-prod/PhysObjectExtractor))

This package runs on CMSSW, meaning that the objective of this package is to extract information from the collisions. These can be electrons, muons, etc. For this workshop we used data from 2015.

The branch for production has some changes so it works for the workshop, plus some scripts.

·      Merge_trees.py: Makes the ntuple be flat

## Analysis

[GitHub]([https://cms-opendata-workshop.github.io/workshop2022-lesson-ttbarljetsanalysis/01-introduction/](https://cms-opendata-workshop.github.io/workshop2022-lesson-ttbarljetsanalysis/01-introduction/)):

Coffea is a library software created by HEP scientists

The idea of this analysis was to do a Columnar analysis, meaning that we are not going event-per-event, rather we will do it all at once, as it is done in the industry.