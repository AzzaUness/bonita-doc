
1.10 Reporting overview
=======================

This page presents an overview of reporting with Bonita BPM.

Terminology
-----------

Reporting is the act of collecting and aggregating Key Performance Indicators (KPIs) to present them in reports.\
KPIs are defined as a set of measures that are used to assess the operational success of a business process.

Reporting is also know as Business Intelligence (BI) or Business Activity Monitoring (BAM).

Reporting dependencies
----------------------

Reporting involves Bonita BPM and two other third-party software components:

-   A reporting database of your choice.
-   A BI tool of your choice.

Reporting main steps
--------------------

Reporting is composed in several ordered steps.

At design time:

1.  [Reporting database setup](/set-reporting-database-0)
2.  [KPI definition in Bonita BPM Studio](/set-kpis-0)
3.  [Report creation in the BI tool](/create-report-0)

At execution time:

1.  KPI collection in Bonita BPM Engine and storage in the reporting database (this is performed automatically based on KPI definitions)
2.  Report generation using the BI tool or Bonita BPM Portal

