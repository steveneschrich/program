# PROGRAM: PROgram GRAnt Management

Large program-style NIH grants often require a tracking or management component associated with reporting on outcomes within the research program. This can involve a significant amount of work tracking grants submitted or papers published based on work funded by the research program. These grants mechanisms often involve cores which provide scientific support to program activities; tracking their contributions is important here as well. 

This software (PROGRAM) is designed to enable tracking and ultimately management of program grants with the goal of providing robust reporting tools for understanding outcomes and productivity associated with research program activity. The overall structure consists of a series of REDCap-based data entry forms (with options for automated population from existing sources) and reporting software. The approach is to consider an academic artifact (grant submission, publication) and provide a mechanism for program-grant specific annotations (e.g., supporting cores, designation within the program grant, investigator status within the program grant). More specifics are included below.

# Getting Started
This repository is a work in progress. Getting started includes deploying a number of servers/software including:
- REDCap
- R with necessary libraries, including latex, etc.
- Host for running docker containers

# Software Components

## PROGRAM-IN - Program Grant Management - Intake and Notation (REDCap-based input)
REDCap is the primary data entry component for the system. With an existing REDCap server, several projects must be imported:
- Grants
- Manuscripts (publications in process)
- Publications (published manuscripts)

NOTE: These projects will need API-access which requires generating API tokens.

## pgrt - program grant reporting tool (R-based reporting)
There is a R library for loading (from REDCap), manipulating (filter, tally) and formatting grants, publications and manuscripts for reporting purposes. Several default templates (with parameters) have been defined within this library.

## PROGRAM-PMI - Program Grant Management - PubMed Importer
This module is a web-based application for importing citations from pubmed in order to annotate.

# Caveats
As noted above, this project is a work in process. Therefore, not all software is currently ready for production usage. Please contact me (steven.eschrich@gmail.com) with questions or ideas.
