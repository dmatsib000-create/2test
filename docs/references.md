# References & Verification Ledger

This file is the single source of truth for every citation that appears anywhere in `2test.html`, `docs/`, or commit messages. Rule: **adding a citation in any tracked file requires updating this file in the same commit.**

Each entry has a stable key (used in code comments and renderer cite-fields), a full citation, and a verification status. Pending entries are best-effort drafts that need clinician sign-off before they back any clinical output.

---

## Verification Ledger

| Key                  | Cited in (file)            | Verified by | Date verified | Status   | Notes |
|----------------------|----------------------------|-------------|---------------|----------|-------|
| hyman2020            | docs/, planned: 2test.html | —           | —             | pending  | Clinical workflow & ASD evaluation framing |
| volkmar2014          | docs/, planned: 2test.html | —           | —             | pending  | AACAP practice parameter |
| dsm5tr               | docs/, planned: 2test.html | —           | —             | pending  | Diagnostic criteria authority |
| aaidd2021            | docs/, planned: 2test.html | —           | —             | pending  | ID tri-criterion |
| greenspan2017        | docs/                      | —           | —             | pending  | BIF construct concerns |
| pitts-mervis-2016    | docs/                      | —           | —             | pending  | KBIT-2 floor effects — exact citation needs verify |
| scq-rutter-2003      | planned                    | —           | —             | pending  | SCQ cutoffs |
| mchat-robins-2014    | planned                    | —           | —             | pending  | M-CHAT-R/F validation |
| scared-birmaher-1999 | planned                    | —           | —             | pending  | SCARED reliability |
| phq-a-johnson-2002   | planned                    | —           | —             | pending  | PHQ-A validation |
| gad7-spitzer-2006    | planned                    | —           | —             | pending  | GAD-7 |
| vanderbilt-wolraich-2003 | planned                | —           | —             | pending  | Vanderbilt psychometrics |
| snap-iv-swanson-1992 | planned                    | —           | —             | pending  | SNAP-IV original |
| aq10-allison-2012    | planned                    | —           | —             | pending  | AQ-10 |
| assq-ehlers-1999     | planned                    | —           | —             | pending  | ASSQ |
| mdq-hirschfeld-2000  | planned                    | —           | —             | pending  | MDQ adult; needs adolescent-adaptation note |
| crafft-knight-2002   | planned                    | —           | —             | pending  | CRAFFT (N-update separate) |
| mchat-rf-cutoffs     | planned                    | —           | —             | pending  | uses mchat-robins-2014 cutoffs |
| nih-asq-horowitz-2012| planned                    | —           | —             | pending  | NIH ASQ (Ask Suicide-Screening Questions) |
| mfq-angold-1995      | planned                    | —           | —             | pending  | Mood and Feelings Questionnaire |
| pq16-ising-2012      | planned                    | —           | —             | pending  | Prodromal Questionnaire-16 |
| rita-t-choueiri-2015 | planned                    | —           | —             | pending  | RITA-T autism toddler screener |
| srs2-constantino-2012| planned                    | —           | —             | pending  | Social Responsiveness Scale, 2nd ed. |
| gars3-gilliam-2014   | planned                    | —           | —             | pending  | Gilliam Autism Rating Scale, 3rd ed. |
| idea-part-c          | planned                    | —           | —             | pending  | Early Steps eligibility basis |
| idea-part-b          | planned                    | —           | —             | pending  | IEP / Child Find basis |
| fla-early-steps      | planned                    | —           | —             | pending  | Florida Early Steps statute |
| fla-beess            | planned                    | —           | —             | pending  | Florida ESE framework |
| section-504          | planned                    | —           | —             | pending  | 504 plan basis |
| ada-title-iii        | planned                    | —           | —             | pending  | Private school accommodations |

When you verify an entry, replace `—` with your initials and the date, and change Status to `verified`. If a citation turns out to be wrong, mark Status `superseded` and add the replacement entry below it — do not delete history.

---

## Clinical authority — autism diagnosis & evaluation

**hyman2020**
Hyman SL, Levy SE, Myers SM; Council on Children with Disabilities, Section on Developmental and Behavioral Pediatrics. Identification, Evaluation, and Management of Children With Autism Spectrum Disorder. *Pediatrics*. 2020;145(1):e20193447.
Used for: overall workflow of ASD evaluation; intake field selection; A&P framing.

**volkmar2014**
Volkmar F, Siegel M, Woodbury-Smith M, King B, McCracken J, State M; American Academy of Child and Adolescent Psychiatry (AACAP) Committee on Quality Issues (CQI). Practice parameter for the assessment and treatment of children and adolescents with autism spectrum disorder. *J Am Acad Child Adolesc Psychiatry*. 2014;53(2):237-257.
Used for: instrument selection rationale; DBP synthesis logic.

**dsm5tr**
American Psychiatric Association. *Diagnostic and Statistical Manual of Mental Disorders, Fifth Edition, Text Revision (DSM-5-TR)*. American Psychiatric Publishing; 2022.
Used for: ASD criteria A-E checklist; severity-level descriptors (SC & RRB, levels 1-3); GDD vs ID distinction; specifier vocabulary.

---

## Clinical authority — intellectual disability & related constructs

**aaidd2021**
Schalock RL, Luckasson R, Tassé MJ. *Intellectual Disability: Definition, Diagnosis, Classification, and Systems of Supports*. 12th ed. American Association on Intellectual and Developmental Disabilities; 2021.
Used for: tri-criterion (IQ deficit + adaptive deficit + developmental onset) for ID confirmation logic.

**greenspan2017**
Greenspan S. Borderline intellectual functioning: an update. *Curr Opin Psychiatry*. 2017;30(2):113-122.
Used for: rationale for NOT auto-bridging any predicate to BIF (R41.83). BIF is an ICD clinical-attention code, not a DSM diagnosis, and Greenspan argues against algorithmic assignment.

**pitts-mervis-2016**
Pitts CH, Klein-Tasman BP, Mervis CB. KBIT-2 floor effects in young children (full citation pending verification).
Used for: screener-young warning when KBIT-2R is applied to children near its lower age boundary.

---

## Instrument manuals & validation papers (auto-interpreted set)

Auto-interpretation only attaches to instruments with published, deterministic cutoffs. Judgment-heavy instruments (DP-4, ADOS-2, CARS-2-ST, KBIT-2R, RITA-T, DAYC-2, SRS-2, GARS-3) are entered raw and the clinician writes prose.

**scq-rutter-2003**
Rutter M, Bailey A, Lord C. *The Social Communication Questionnaire Manual*. Western Psychological Services; 2003.
Used for: SCQ total cutoff ≥15 → above autism screening threshold.

**mchat-robins-2014**
Robins DL, Casagrande K, Barton M, Chen CA, Dumont-Mathieu T, Fein D. Validation of the Modified Checklist for Autism in Toddlers, Revised With Follow-up (M-CHAT-R/F). *Pediatrics*. 2014;133(1):37-45.
Used for: M-CHAT-R/F low/medium/high-risk cutoffs and follow-up logic.

**scared-birmaher-1999**
Birmaher B, Brent DA, Chiappetta L, Bridge J, Monga S, Baugher M. Psychometric properties of the Screen for Child Anxiety Related Emotional Disorders (SCARED): a replication study. *J Am Acad Child Adolesc Psychiatry*. 1999;38(10):1230-1236. (See also: Birmaher B, et al. 1997, original instrument.)
Used for: SCARED total ≥25 and subscale cutoffs.

**phq-a-johnson-2002**
Johnson JG, Harris ES, Spitzer RL, Williams JBW. The Patient Health Questionnaire for Adolescents: validation of an instrument for the assessment of mental disorders among adolescent primary care patients. *J Adolesc Health*. 2002;30(3):196-204.
Used for: PHQ-9A scoring bands (minimal/mild/moderate/moderately severe/severe).

**gad7-spitzer-2006**
Spitzer RL, Kroenke K, Williams JBW, Löwe B. A brief measure for assessing generalized anxiety disorder: the GAD-7. *Arch Intern Med*. 2006;166(10):1092-1097.
Used for: GAD-7 scoring bands.

**vanderbilt-wolraich-2003**
Wolraich ML, Lambert W, Doffing MA, Bickman L, Simmons T, Worley K. Psychometric properties of the Vanderbilt ADHD diagnostic parent rating scale in a referred population. *J Pediatr Psychol*. 2003;28(8):559-567.
Used for: Vanderbilt parent/teacher inattentive, hyperactive/impulsive, ODD, CD, anxiety/depression score cutoffs and performance impairment threshold.

**snap-iv-swanson-1992**
Swanson JM. *School-Based Assessments and Interventions for ADD Students*. KC Publishing; 1992. (See also Swanson JM, et al. 2012 methods paper for the SNAP/SWAN rating scales.)
Used for: SNAP-IV item-mean cutoffs for inattentive and hyperactive/impulsive domains.

**aq10-allison-2012**
Allison C, Auyeung B, Baron-Cohen S. Toward brief "Red Flags" for autism screening: the Short Autism Spectrum Quotient and the Short Quantitative Checklist in 1,000 cases and 3,000 controls. *J Am Acad Child Adolesc Psychiatry*. 2012;51(2):202-212.
Used for: AQ-10 ≥6 referral threshold.

**assq-ehlers-1999**
Ehlers S, Gillberg C, Wing L. A screening questionnaire for Asperger syndrome and other high-functioning autism spectrum disorders in school age children. *J Autism Dev Disord*. 1999;29(2):129-141.
Used for: ASSQ parent/teacher cutoffs.

**mdq-hirschfeld-2000**
Hirschfeld RMA, Williams JBW, Spitzer RL, et al. Development and validation of a screening instrument for bipolar spectrum disorder: the Mood Disorder Questionnaire. *Am J Psychiatry*. 2000;157(11):1873-1875.
Used for: MDQ adult thresholds. WARY: when used in adolescents, attach the Wagner 2006 adaptation note before any auto-interpretation prose is generated.

**crafft-knight-2002**
Knight JR, Sherritt L, Shrier LA, Harris SK, Chang G. Validity of the CRAFFT substance abuse screening test among adolescent clinic patients. *Arch Pediatr Adolesc Med*. 2002;156(6):607-614.
Used for: CRAFFT score ≥2 referral threshold. CRAFFT-N (nicotine update) cite to be added when verified.

**nih-asq-horowitz-2012**
Horowitz LM, Bridge JA, Teach SJ, et al. Ask Suicide-Screening Questions (ASQ): a brief instrument for the pediatric emergency department. *Arch Pediatr Adolesc Med*. 2012;166(12):1170-1176.
Used for: NIH ASQ — any yes on items 1-4 indicates a non-acute positive screen; yes on item 5 (current thoughts) indicates acute risk requiring immediate safety evaluation.

**mfq-angold-1995**
Angold A, Costello EJ, Messer SC, Pickles A, Winder F, Silver D. The development of a short questionnaire for use in epidemiological studies of depression in children and adolescents. *Int J Methods Psychiatr Res*. 1995;5:237-249.
Used for: MFQ-Child short form total ≥12 (or long form ≥27) suggests depressive symptoms warranting follow-up.

**pq16-ising-2012**
Ising HK, Veling W, Loewy RL, et al. The validity of the 16-item version of the Prodromal Questionnaire (PQ-16) to screen for ultra high risk of developing psychosis in the general help-seeking population. *Schizophr Bull*. 2012;38(6):1288-1296.
Used for: PQ-16 ≥6 endorsed items indicates a positive screen for prodromal/ultra-high-risk psychosis features warranting specialty referral.

**rita-t-choueiri-2015**
Choueiri R, Wagner S. A new interactive screening test for autism spectrum disorders in toddlers. *J Pediatr*. 2015;167(2):460-466.
Used for: RITA-T total ≥10 indicates positive autism screen in toddlers 18-36 months.

**srs2-constantino-2012**
Constantino JN, Gruber CP. *Social Responsiveness Scale, Second Edition (SRS-2) Manual*. Western Psychological Services; 2012.
Used for: SRS-2 total T-score bands: <60 within normal limits, 60-65 mild, 66-75 moderate, ≥76 severe range associated with clinically significant social communication impairment.

**gars3-gilliam-2014**
Gilliam JE. *Gilliam Autism Rating Scale, Third Edition (GARS-3) Manual*. PRO-ED; 2014.
Used for: GARS-3 Autism Index categorical bands: ≥85 "Very likely" probability of autism; 71-84 "Probable"; ≤70 "Unlikely."

---

## Legal & regulatory — IEP / school letter templates

**idea-part-c**
Individuals with Disabilities Education Act, Part C: Infants and Toddlers with Disabilities. 20 U.S.C. §§1431-1444; 34 C.F.R. Part 303.
Used for: Early Steps referral letter template (under-3 status).

**idea-part-b**
Individuals with Disabilities Education Act, Part B: Assistance for Education of All Children with Disabilities. 20 U.S.C. §§1411-1419; 34 C.F.R. Part 300, especially §300.111 (Child Find) and §300.324 (development, review, and revision of IEP).
Used for: Child Find evaluation request letters; IEP amendment/inadequacy letters.

**fla-early-steps**
Florida Early Steps Program. Fla. Stat. §§391.301-391.308.
Used for: Florida-specific framing in under-3 referral letter template.

**fla-beess**
Florida Bureau of Exceptional Education and Student Services (BEESS). Fla. Stat. Ch. 1003, Part IV. (BEESS guidance URL pending verification.)
Used for: accommodation vocabulary; ESE service framing in school-age letter templates.

**section-504**
Rehabilitation Act of 1973, Section 504. 29 U.S.C. §794; 34 C.F.R. Part 104.
Used for: 504-plan letter framing; 504-to-IEP upgrade letter rationale.

**ada-title-iii**
Americans with Disabilities Act, Title III: Public Accommodations and Commercial Facilities. 42 U.S.C. §§12181-12189.
Used for: private-school accommodation letter framing where IDEA does not bind.

---

## How to add a new citation

1. Add the verification-ledger row with Status `pending`.
2. Add the full citation block under the appropriate section above.
3. Use the stable key wherever you cite it in code or other docs.
4. Verify (or have verified) before any clinical output relies on it. Update the ledger row.
