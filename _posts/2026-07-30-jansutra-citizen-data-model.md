---
layout: post
title: "Jansutra: A Semantic Data Model for India's Citizen Welfare Data"
date: 2026-07-30 00:00:00 +0530
categories: policy engineering
author: "Abhishek Shah"
tags: india policy data semantic-web ontology open-source
---

A few days ago I wrote about [India's welfare data problem](https://abhishek-shah.dev/blog/2026/07/26/india-needs-central-database/). The short version: India's flagship welfare programs are targeted using SECC 2011 data that is 15 years old, ministry registers cannot be joined because they have no common key, and the most marginalised households — the ones sitting at the intersection of multiple deprivations — are invisible to any system designed to help them.

I used one household as the running example. An ST woman in Bandgaon village, West Singhbhum district, Jharkhand. BPL, five-person household, agricultural labourer. Holds a Forest Rights Act title to 0.8 hectares of forest land — a paper asset she cannot leverage for credit. No safe water source within 4.5 km. Nearest PHC is 18.5 km away, nearly double the NHM's 10 km norm. One child under five. MGNREGA job card. PM-JAY coverage.

Every government scheme has something to offer her. No government system can find her.

So I built a sketch of what the alternative could look like.

---

## A data model, not a database

The first thing to say is what Jansutra is not: it is not a central database. That framing is both politically toxic and architecturally wrong for India's context. The country's bureaucratic geography — twelve ministries, twenty-eight states, a history of data collection exercises being captured by local power structures — makes a single monolithic welfare database impossible to build and dangerous to operate.

What Jansutra is instead is a **semantic data model**: a shared vocabulary that lets existing ministry registers speak to each other without requiring them to merge. Think of it the way HTTP works. HTTP did not centralise the internet; it gave every server a common protocol so a browser built in California could retrieve a page served from a rack in Mumbai. Jansutra tries to do the same thing for India's welfare data: define the vocabulary, not own the store.

The technical implementation is an OWL ontology serialised as JSON-LD, with SHACL validation shapes and Python tooling. None of that matters to a policy audience. What matters is that the model defines how caste categories, poverty classifications, land tenure types, disability categories, and service access levels are named and related — in a way that any ministry's ETL pipeline could adopt, and that would make the resulting data queryable across registers.

---

## What the model looks like

The ontology has seven modules, each a separate file.

**Social**: caste category (SC/ST/OBC-C/OBC-S/EWS/NT/DNT/GEN using constitutional categories, not jati), gender identity aligned with NALSA 2014, Census 2011 religion codes, mother tongue as a BCP47 language tag.

**Economic**: annual household income, monthly per-capita expenditure (the welfare economics proxy), poverty status (BPL/APL/AAY), occupation type from SECC's livelihood categories, household size, MGNREGA job card.

**Geography**: India's four-level administrative hierarchy — State/UT → District → Sub-district → Village/Ward. Every administrative unit carries a mandatory MoPR Local Government Directory code. More on why LGD specifically in a moment.

**Land**: land parcel area, type (irrigated, dryland, homestead, forest land under FRA, fallow), tenure (owned-titled, owned-untitled for customary/tribal claims, leased, sharecropped, encroached, communal commons), title deed status, and the khasra number for cross-referencing with state revenue records.

**Health**: disability type under RPWD Act 2016, disability benchmark percentage, PM-JAY coverage, PMAY beneficiary status, pregnant or lactating at survey time, children under five. This is not a clinical health record — it captures welfare-eligibility dimensions only.

**Services**: eleven service types — piped water, safe water source, grid electricity, household toilet, paved road, broadband internet, primary school, secondary school, PHC, Anganwadi centre, bank branch — each with a quality rating (reliable / intermittent / absent) and, where relevant, a walking distance.

**Core**: the `Citizen` class that links everything together.

The citizen in Bandgaon looks like this in the model's data format:

```json
{
  "@id": "citizen:f7a3d2b1-4e89-4c56-a123-9b0c12de3f45",
  "@type": "jansutra:Citizen",
  "jansutra:residesIn": {"@id": "geo:village/jharkhand/west-singhbhum/chakradharpur/bandgaon"},
  "jansutra:hasSocialRecord": {"@id": "citizen:f7a3d2b1-.../social"},
  "jansutra:hasServiceAccessRecord": {"@id": "citizen:f7a3d2b1-.../services"}
}
```

```json
{
  "@id": "citizen:f7a3d2b1-.../services/water",
  "@type": "jansutra:ServiceAccess",
  "jansutra:accessType": {"@id": "jansutra:SafeWaterSource"},
  "jansutra:accessQuality": {"@id": "jansutra:Absent"},
  "jansutra:distanceInKm": {"@type": "xsd:decimal", "@value": "4.5"}
}
```

The full model, with all seven modules, SHACL validation shapes, three SPARQL queries, and a complete example data file, is open source at [github.com/ashah2012/jansutra](https://github.com/ashah2012/jansutra). Documentation at [ashah2012.github.io/jansutra](https://ashah2012.github.io/jansutra/).

---

## The query that motivated everything

Once the model is populated with real survey data, finding the Bandgaon household becomes this:

```sparql
SELECT ?citizen ?districtLabel ?povertyStatus ?phcDistanceKm
WHERE {
  ?citizen a jansutra:Citizen ;
           jansutra:residesIn ?village ;
           jansutra:hasSocialRecord ?social ;
           jansutra:hasEconomicRecord ?economic ;
           jansutra:hasServiceAccessRecord ?services .

  ?social jansutra:casteCategory jansutra:ST .
  ?village jansutra:isRemoteArea true .

  ?village jansutra:hasParentUnit* ?district .
  ?district jansutra:adminLevel jansutra:District ;
            skos:prefLabel ?districtLabel .
  FILTER(LANG(?districtLabel) = "en")

  FILTER NOT EXISTS {
    ?services jansutra:hasServiceAccess ?waterAccess .
    ?waterAccess jansutra:accessType ?waterType ;
                 jansutra:accessQuality ?wq .
    FILTER(?waterType IN (jansutra:PipedWater, jansutra:SafeWaterSource))
    FILTER(?wq IN (jansutra:Reliable, jansutra:Intermittent))
  }

  OPTIONAL {
    ?services jansutra:hasServiceAccess ?phcAccess .
    ?phcAccess jansutra:accessType jansutra:PHCAccess ;
               jansutra:distanceInKm ?phcDistanceKm .
  }

  OPTIONAL {
    ?economic jansutra:povertyStatus ?ps .
    ?ps skos:notation ?povertyStatus .
  }
}
ORDER BY ?districtLabel ?citizen
```

In plain English: find every Scheduled Tribe citizen in a remote village who has no functional water access — no reliable piped water, no reliable safe water source — and show me their district, poverty status, and how far they are from the nearest PHC.

Against the example data file, this returns one row in milliseconds: West Singhbhum, BPL, 18.5 km. Against a national dataset, this would give Jal Jeevan Mission a priority ranking of every habitation to hit next year, weighted by the socioeconomic profile of the households still unserved.

The `jansutra:hasParentUnit*` part is doing something important: it walks up the administrative hierarchy — village to sub-district to district — so you can ask the question at district level regardless of what level the citizen's residence is recorded at. This is the kind of thing that is trivially expressible in a graph query language and genuinely painful to do with a relational join across tables that have different administrative unit conventions.

---

## Three design choices worth explaining

**No Aadhaar.** Citizens in the model are identified by opaque UUID URIs — `citizen:f7a3d2b1-...`, with nothing embedded in the identifier. No name, no Aadhaar number, no biometric reference.

The reason is not just privacy sentiment. A profile that links ST + BPL + disability + no-water access + FRA land title is a high-resolution vulnerability fingerprint. If that profile were tied to an Aadhaar number, it would become trivially re-identifiable by anyone with access to the UIDAI registry. The resulting dataset would be a surveillance instrument — exactly the kind of thing that made the SECC's OBC data politically impossible to release.

UUID-based identification keeps the identity mapping outside the model. A ministry that needs to send DBT transfers to the UUID cohort it extracts from a Jansutra query would hold an internal Aadhaar ↔ UUID lookup table, audited separately. Jansutra itself never becomes an Aadhaar database. It also keeps Jansutra outside the Aadhaar Act 2016's scope, which has its own restrictions on purpose and use.

**LGD codes as the geographic spine.** Every administrative unit in the model carries a mandatory MoPR Local Government Directory code. Not a Census village code. Not a state revenue survey number. The LGD code.

Census village codes change between decennial surveys — villages split, merge, are reclassified as urban areas, and when that happens the old code becomes invalid. Data collected in one census cycle cannot be joined with the next without a remapping exercise. LGD codes are explicitly designed for ongoing administrative use and are stable across census cycles.

More importantly, LGD codes are already used by MGNREGA, PMGSY, Jal Jeevan Mission, Swachh Bharat Mission, UDISE, and most of the central government's MIS systems. The interoperability key already exists. Jansutra makes it the primary join field.

**Federated modules.** The seven modules are independent JSON-LD files, linked by a shared vocabulary. The Ministry of Tribal Affairs can make available a land module — FRA title data — to MoRD for housing scheme targeting without exposing health records or income data. A state government can deploy only geography and economic modules. The model does not require a central data store; it requires a common vocabulary.

This is the architecture the previous post called "sector-specific datastores." The model is the specification of what those datastores should contain and how they should name things. The actual deployment — where the data lives, who can read it, how it is governed — is the deployer's problem.

---

## What this isn't

One example household does not make a national database. Let me be precise about the gap.

The Jansutra repository contains one data file: the Bandgaon citizen, constructed to illustrate the model. Running the vulnerability query against it returns one row. The query that would tell you how many ST households in every district of India lack water access does not exist yet, because the data to run it against does not exist.

That data would require ETL pipelines from SECC (poverty, occupation, MGNREGA), the Census 2026 caste enumeration (caste and gender), DILRMP state land registries (land and tenure), NFHS-6 or state HMIS (health dimensions), and the JJM, SBM-G, PMGSY, and BharatNet dashboards (service access). None of those feeds are built. Someone with access to those datasets and the budget to build those pipelines would need to do that work.

The model also does not execute welfare delivery. Jansutra can tell you which UUID cohort is BPL + ST + no-water in a given district. It cannot send them money, issue ration cards, or prioritise their village for road construction. That is still the ministry's job, through its own systems. Jansutra is the indexing layer, not the delivery layer.

And the data quality problem the previous post described applies to any implementation too. SECC 2011 is 15 years old. NFHS-6 is a district-level sample survey, not individual records for every household. The model can represent current data if current data exists; the data itself is the hard part.

---

## What it would take to matter

Census 2026 is the first real brick. India approved caste enumeration for the 2026 Census in April 2025. When that enumeration is conducted and the data released, Jansutra has a SKOS vocabulary ready to receive it: SC, ST, OBC-Central, OBC-State, EWS, NT, DNT, General. That data could populate the social module.

The land module can receive data from DILRMP and the MoTA FRA dashboard today — both are digitised to varying degrees depending on state. Some states (Jharkhand, Karnataka) are well along on land record digitisation. Others are not. The khasra number in the land module is the cross-reference key.

The governance prerequisite — which is not an engineering problem — is an independent body with statutory standing and a mandate to maintain and refresh the database on a regular cycle. Not a ministry department that can be starved of budget or folded into another ministry by the next government. Something like the Unique Identification Authority model but with a data stewardship mandate, not just an identity mandate.

The technical problem is tractable. The previous post ended with the observation that "the harder work — building the systems, the governance frameworks, and the political will to use this data honestly — is still ahead." That remains true. Jansutra proves the vocabulary exists and the queries work. It does not solve the political economy that kept SECC 2011's OBC data unreleased for 15 years.

---

## An open invitation

The model is at [github.com/ashah2012/jansutra](https://github.com/ashah2012/jansutra), MIT licensed, with full documentation at [ashah2012.github.io/jansutra](https://ashah2012.github.io/jansutra/).

If you work in a state government or a ministry and have access to any of the source datasets — SECC, NFHS unit records, DILRMP extracts, FRA title data — and want to prototype an ETL pipeline, I would like to hear from you. If you are a policy researcher who wants to ask specific questions that this model should be able to answer but currently cannot, I would like to hear that too. The model is opinionated but not fixed.

India has about 640 districts, roughly 100 million rural households, and a Census happening this year that will produce caste data for the first time in nearly a century. The infrastructure to make that data queryable in the way described above could exist. The question, as always, is whether the political will to use it honestly will follow.
