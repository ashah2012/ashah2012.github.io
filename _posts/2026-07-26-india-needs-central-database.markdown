---
layout: post
title: "India's Missing Map: The Case for a Unified Socioeconomic Database"
date: 2026-07-26 00:00:00 +0530
categories: policy
author: "Abhishek Shah"
tags: india policy data governance
---

India has Aadhaar — the world's largest biometric identity system, with 1.38 billion enrollments. It is an engineering marvel: a unique 12-digit number backed by fingerprints and iris scans, issued to nearly every resident of the country. Yet for all its scale, Aadhaar cannot tell you how many OBC farmers in Vidarbha are living below the poverty line, or whether an ST household in Jharkhand has access to clean water. Identity and socioeconomic reality are two entirely different maps — and India has only one of them.

## What India Has Today

To be fair, India is not completely in the dark. It has several systems, each capturing a slice of the picture.

**Aadhaar** handles identity. The UIDAI database stores name, date of birth, gender, address, and biometrics. By design and by law, it does not store caste, religion, economic status, or health data. This was an intentional architectural choice — Aadhaar was meant to be a minimal identifier, not a citizen profile.

**The Socio-Economic and Caste Census (SECC) of 2011** was a more ambitious exercise. Conducted door-to-door across rural and urban India, it attempted to map households by their economic condition and caste identity. The SC and ST data was released. The OBC caste data — covering the largest share of the population — was quietly suppressed and has never been made public, largely due to the political firestorm it would ignite around reservation arithmetic.

**The decennial Census** captures SC and ST populations but, since 1931, has not asked about caste for the general population. The 2021 Census was postponed due to COVID and has still not been conducted. Census 2026 has now been announced, and crucially, the government approved caste enumeration in April 2025 — the first time India will count castes since British rule. That is genuinely significant. But a once-a-decade headcount is not a living database; it is a photograph.

**State-level surveys** have tried to fill the gap. Bihar conducted a caste survey in 2023. Telangana completed one of the most comprehensive caste-socioeconomic surveys in independent India's history in 2024-25. Karnataka has done its own count. These are valuable. They are also incomparable across states, methodologically inconsistent, and politically contested.

What we have, then, is five siloed systems — UIDAI, the Registrar General's Census, SECC, the National Population Register, and state-level welfare databases — that largely do not speak to each other, are anchored to different points in time, and collectively still leave large blind spots.

## The Cost of Flying Blind

This is not an abstract problem. The data vacuum has real, documentable consequences.

India's flagship welfare programs — PDS ration cards, PM-KISAN, housing schemes, health insurance — are targeted using SECC 2011 data. That data is now 15 years old. Households that fell into poverty after 2011 are invisible to this system. Children who were born into deprivation during the pandemic, families displaced by floods, agricultural workers who lost livelihoods to mechanisation — none of them exist in the targeting database.

The Aadhaar-DBT (Direct Benefit Transfer) pipeline was supposed to modernise delivery. In many ways it has — ghost beneficiaries and duplication have been reduced significantly. But a different kind of exclusion has emerged: biometric authentication failures. Manual labourers whose fingerprints are worn smooth by work, elderly residents whose irises have changed, people in areas with poor connectivity — these are the groups that Aadhaar-based authentication fails most often. They are, by definition, among the most vulnerable.

The sub-categorisation problem within OBCs is perhaps the starkest example of what bad data costs. The Supreme Court ruled in 2024 that states can sub-categorise within OBCs to ensure that the most marginalised groups within that category actually benefit from reservations. The Rohini Commission, set up in 2017, had identified this issue years earlier — a handful of dominant OBC communities were cornering most reservation benefits while smaller, more marginalised groups received almost nothing. But the Commission could not fully quantify the problem because the underlying population data did not exist. We know the injustice is happening. We cannot measure how much, or to whom, or where.

Five out of every six multidimensionally poor Indians belong to SC, ST, or OBC categories. But without granular, current data, allocation of resources across and within these groups remains educated guesswork.

## What a Unified Database Would Unlock

The case for building this infrastructure is not just about plugging gaps. It is about what becomes possible.

**Precision welfare delivery.** Rather than targeting households based on a 15-year-old survey, schemes could use a dynamic, regularly updated dataset. The right household gets the right scheme, and when circumstances change, the database reflects that.

**Evidence-based reservation policy.** Reservations in India are still calibrated against population estimates that trace back to 1931 census data and political negotiations since. A current, verified count of caste populations — as Census 2026 aims to provide — would give policymakers actual numbers to work with. Sub-categorisation within OBCs becomes empirically tractable rather than politically improvised.

**Tracking whether policy is working.** Are reservations generating social mobility? Is the gap in land ownership between ST and non-ST households closing? Are OBC children completing higher education at higher rates than a decade ago? These are questions India currently cannot answer with rigour, because we lack the longitudinal data. A live database makes this kind of accountability possible.

**Intersectional vulnerability mapping.** The worst-off people in India are not just poor, or just from a marginalised caste, or just women, or just in a remote district — they are all of these things at once. A unified database that links economic status, caste identity, gender, geography, and access to services would let planners identify where these vulnerabilities compound, and direct resources there.

## Why It Hasn't Been Built

The obstacles are real, and worth taking seriously rather than dismissing.

The political sensitivity of caste data is enormous. Releasing OBC caste counts reshapes the arithmetic of reservation politics. Powerful OBC communities fear losing share; smaller communities fear being lumped in with those who already captured the benefits. Any government that releases granular caste data immediately makes enemies. This is why SECC 2011's OBC data sat in a drawer for 15 years.

Privacy and surveillance concerns are legitimate. Aadhaar has already attracted serious criticism from civil society for enabling the state to track citizens across domains in ways that were not originally sanctioned. Centralising caste and economic data on top of a biometric identity system raises the spectre of a surveillance infrastructure with no parallel in the democratic world.

There is also the bureaucratic fragmentation problem. UIDAI reports to the Ministry of Electronics and IT. The Census is the Registrar General's office under Home Ministry. Caste welfare falls under Social Justice. Nobody owns the integration problem, and nobody has the political capital to force it through.

Finally, the logistical scale is genuinely daunting: 1.4 billion people, 22+ scheduled languages, massive rural infrastructure deficits, and a history of data collection exercises being captured by local power structures.

## How to Do It Right

None of these obstacles are insurmountable. The question is whether to try.

Start with what Census 2026 will produce. The approved caste enumeration is the first solid brick. Build the methodology carefully, release the data publicly in aggregate, and establish it as the new baseline.

Use a **federated architecture**. Aadhaar already works as a linking key across systems. The database does not need to be a single monolith under one ministry — it can be sector-specific datastores (health, economic status, caste, land ownership) that can be linked via Aadhaar when authorised, but are otherwise managed separately. This limits the surveillance surface significantly.

Use the **Digital Personal Data Protection Act (2023)** as the legal scaffolding. India now has a data protection framework. Apply it: individual records must be protected, purpose limitation must be enforced, public access is to aggregates only, and re-identification must be treated as a serious offence.

Build in **re-survey cycles**. The SECC disaster happened partly because it was treated as a one-time exercise that was then frozen in time. A database is infrastructure, not a project. It needs regular refresh cycles — every 5 years, ideally linked to the Census cycle.

Establish an **independent oversight body** with constitutional or statutory standing. Not a ministry department. Not a commission that can be dissolved by the next government. Something with tenure and authority, like the Election Commission model.

## The Social Accounting India Owes Itself

This is not a call for surveillance. It is a call for honesty.

India's Constitution promises equality before the law and equal protection of the law. The state's affirmative action programs are premised on the idea that historical discrimination can be identified, measured, and addressed. But you cannot engineer equality without measuring inequality. You cannot target deprivation if you don't know where it lives or whose face it wears.

A map is not the territory. But without a map, you are lost — and worse, you are making consequential decisions about the lives of 1.4 billion people while lost. India's welfare state, for all its ambition, is currently navigating by a 15-year-old photograph.

Census 2026 is a beginning. The harder work — building the systems, the governance frameworks, and the political will to use this data honestly — is still ahead. The question is whether India will treat this as the infrastructure project it actually is, or whether the data will end up in the same drawer as SECC 2011's OBC findings.

The country's most marginalised people deserve better than that.
