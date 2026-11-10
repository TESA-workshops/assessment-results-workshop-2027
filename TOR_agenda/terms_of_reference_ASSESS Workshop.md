---
output:
  html_document: default
  word_document: default
---
# 2027 TESA Assessment Database Workshop - Terms of Reference (ToR) 

**Technical Expertise for Stock Assessment (TESA) Workshop**  
**Title:** Developing an Open Database of Canadian Fisheries Stock Assessment Outputs  
**Dates:** January 12-14, 2027  
**Location:** In-person — Bedford Institute of Oceanography (BIO), Dartmouth, Nova Scotia, Canada  
**Co-Chairs:** Catarina Wor (DFO Pacific Region), Jacob Burbank (DFO Gulf Region)  
**Steering/Support Committee:** Robyn Forrest (DFO Pacific), Daniel Ricard (DFO Gulf), Sean Anderson (DFO Pacific), Tom Bird (DFO Pacific), Julie Marentette (DFO National Capital Region)  
**Invited Expert:** Carlos Pinto (International Council for the Exploration of the Sea)

---

## 1. Context and Background

Most major international fishing jurisdictions, including the United States (NOAA) and Europe (ICES), provide annual standardized repositories and interactive platforms reporting on the status and trends of fish stocks. 
In contrast, Canada currently lacks a centralized, standardized quantitative database of stock assessment outputs. 
While Canada’s *Sustainability Survey for Fisheries* (SSF) reports nationally on fish stock status, it is primarily text-based, non-queryable, and covers only a subset of assessed stocks.

Recent initiatives within Fisheries and Oceans Canada (DFO) such as the transition toward standardized Fisheries Science Advisory Reports (FSAR) 
present a timely opportunity to institute standardized data collections for all managed stocks. 
Furthermore, adapting Canadian stock assessment outputs into open, queryable formats aligns with national [Open Data directives](https://www.dfo-mpo.gc.ca/about-notre-sujet/publications/science/datapolicy-politiquedonnees/index-eng.html) and responds to growing calls for systematic reporting to Parliament regarding fish stock health and management performance.

International models such as the [ICES AdviceXplorer](https://www.ices.dk/advice/Pages/adviceXplorer.aspx), [NOAA's Stock SMART](https://apps-st.fisheries.noaa.gov/stocksmart?app=homepage), and the global [RAM Legacy Stock Assessment Database (RAMLDB)](http://ramlegacy.org/) 
demonstrate how standardized repositories improve scientific transparency, public accountability, and regional research capacity. 
Integrating Canadian data into a standardized database will increase transparency of stock assessment outputs and will ensure Canadian stock trends are accurately represented in global, multi-stock analyses.

---

## 2. Objectives and Scope

The primary objective of this TESA workshop is to establish a standardized framework, repository architecture, 
and user tool design for storing and querying Canadian stock assessment time series and indicators.

Specifically, the workshop will address the following core objectives:

1. **Standardized Reporting Templates:** Refine, test, and finalize standardized templates for submitting stock assessment outputs across all DFO regions.
2. **Database Population:** Input and validate recent stock assessment data brought by participants to populate the preliminary database.
3. **Interactive Application Conceptualization:** Draft user requirements and mock-up key functional features for an interactive, web-based visualization tool (inspired by ICES AdviceXplorer and NOAA Stock SMART).
4. **Database Architecture & Repository Design:** Review the proposed structure of database and metadata standards required for a national quantitative repository that could align with CSAS FSAR output standards (e.g., standard four-panel stock status figures) and status reports.
5. **Next Steps and Outreach Plan:** Draft a governance plan and clear roadmap for next steps to implement the database nationwide and develop an outreach plan to optimize use. 

---

## 3. Terms of Reference (ToRs) / Work Plan

The workshop will be structured around the following specific tasks (**ToR a – ToR e**):

* **ToR a) Review and Refine Data Templates**
  * Evaluate preliminary reporting templates developed by the co-chairs and steering committee.
  * Agree on key quantitative metrics and how they will be represented in the data templates including, at minimum: time series of stock biomass/abundance, recruitment, total catch/landings, fishing pressure, and reference points.

* **ToR b) Hands-on Data Input and Validation**
  * Process assessment outputs contributed by participants (or assigned from recent CSAS assessments across DFO Regions).
  * Stress-test template functionality across diverse assessment model types (e.g., surplus production, integrated statistical catch-at-age, MSE, empirical/data-limited frameworks).
  * Brainstorm solutions for non-standard assessments and reference points. 

* **ToR c) User Interface & Visualization Tool Design**
  * Identify core functionality, common queries, filtering capabilities, and interactive plotting needs for an end-user web application; include the consideration of providing formatted data outputs for parties interested in obtaining stock status information from Canada.
  * Agree on core visual outputs (plots and tables).
  * Outline technical pathways for deployment (e.g., Shiny/R-based tools, DMApps).

* **ToR d) Database Schema and Data Governance Specifications**
  * Draft data governance rules, submission workflows, version control guidelines, and metadata standards.
  * Draft specifications to enable potential integration into future CSAS publishing workflows (e.g., requiring standardized template submission alongside final FSARs).
  * Establish a governance structure to support further development and incorporation into Open Data, and identify key challenges.

* **ToR e) Develop Plan for Next Steps and Outreach**
  * Collectively draft a plan for the next steps to implement the database nationally.
  * Develop outreach strategy to gain support from senior management and encourage uptake across DFO regions and stock assessors. 

---

## 4. Expected Outputs and Deliverables

1. **Standardized Data Submission Package:** Final draft templates accompanied by detailed user guidance and metric definitions.
2. **Initial Canadian Stock Database:** An initial populated database containing time series and reference points for a target initial suite of Canadian fish stocks.
3. **Functional Requirements Document:** A draft summary document detailing database schema, metadata definitions, and user-interface specifications for the interactive web application.
4. **TESA Workshop Report:** A comprehensive summary report documenting workshop outcomes, template specifications, an outreach plan outlining next steps, and recommendations for long-term operational maintenance and potential CSAS integration.

---

## 5. Targeted Participation

Participation is open to DFO Science staff, stock assessment scientists, data managers, and resource managers across all DFO regions, 
as well as interested collaborators. Participants are requested to bring assessment outputs from their respective regions/stocks to contribute before the workshop and/or during workshop sessions.
