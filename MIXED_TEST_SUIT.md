# Mixed
## Predicated and where

1. Upload `Corona_Anamnese.opt` if not exist
2. Upload `Corona_Anamnese2.opt` if not exist
3. Create ehr and save {ehr_id1}
4. Create composition `Corona_Anamnese.json` and save {comp_id1}
5. Create ehr and save {ehr_id2}
6. Create composition `Corona_Anamnese2.json` and save {comp_id2}
7. Create composition `Corona_Anamnese3.json` and save {comp_id3}
8. Run Query `SELECT e/ehr_id/value, c/uid/value, o/name/value,  l/value/value FROM EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.symptom_sign_screening.v0,'Husten'] contains ELEMENT l[at0005,'Vorhanden?'] where c/archetype_details/template_id/value matches { 'Corona_Anamnese','Corona_Anamnese2'}   and e/ehr_id/value matches {'{ehr_id1}','{ehr_id2}'}`
9. Check Result : ```
   [
   "{ehr_id2}",
   "{comp_id3}",
   "Husten",
   "Vorhanden"
   ],
   [
   "{ehr_id2}",
   "{comp_id2}",
   "Husten",
   "Nicht vorhanden"
   ],
   [
   "{ehr_id1}",
   "{comp_id1}",
   "Husten",
   "Vorhanden"
   ]
   ]```
10. Run Query `SELECT e/ehr_id/value, c/uid/value, o/name/value,  l/value/value FROM EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.symptom_sign_screening.v0,'Husten'] contains ELEMENT l[at0005] where c/archetype_details/template_id/value matches { 'Corona_Anamnese','Corona_Anamnese2'}   and e/ehr_id/value matches {'{ehr_id1}','{ehr_id2}'}`
11. Check Result : ```
    [
    "{ehr_id2}",
    "{comp_id3}",
    "Husten",
    "Vorhanden"
    ],
    [
    "{ehr_id2}",
    "{comp_id2}",
    "Husten",
    "Nicht vorhanden"
    ],
    [
    "{ehr_id1}",
    "{comp_id1}",
    "Husten",
    "Vorhanden"
    ]
    ]```
12. Run Query `SELECT e/ehr_id/value, c/uid/value, o/name/value,  l/value/value FROM EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.symptom_sign_screening.v0,'Husten'] contains ELEMENT l[name/value='Vorhanden?'] where c/archetype_details/template_id/value matches { 'Corona_Anamnese','Corona_Anamnese2'}   and e/ehr_id/value matches {'{ehr_id1}','{ehr_id2}'}`
13. Check Result : ```
    [
    "{ehr_id2}",
    "{comp_id3}",
    "Husten",
    "Vorhanden"
    ],
    [
    "{ehr_id2}",
    "{comp_id2}",
    "Husten",
    "Nicht vorhanden"
    ],
    [
    "{ehr_id1}",
    "{comp_id1}",
    "Husten",
    "Vorhanden"
    ]
    ]```
14. Run Query `SELECT e/ehr_id/value, c/uid/value, o/name/value,  l/value/value FROM EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.symptom_sign_screening.v0,'Husten'] contains ELEMENT l[at0005,'Vorhanden?'] where c/archetype_details/template_id/value matches { 'Corona_Anamnese'}   and e/ehr_id/value matches {'{ehr_id1}'}`
15. Check Result : ```
    [
    "{ehr_id1}",
    "{comp_id1}",
    "Husten",
    "Vorhanden"
    ]
    ]```
16. Run Query `SELECT  o/name/value,  l/value/value FROM EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.symptom_sign_screening.v0] contains ELEMENT l[at0005,'Vorhanden?'] where c/archetype_details/template_id/value matches { 'Corona_Anamnese'}   and e/ehr_id/value matches {'{ehr_id1}'}`
17. Check Result : ```
    [
    "Durchfall",
    "Nicht vorhanden"
    ],
    [
    "Fieber oder erhöhte Körpertemperatur",
    "Vorhanden"
    ],
    [
    "Gestörter Geruchssinn",
    "Nicht vorhanden"
    ],
    [
    "Gestörter Geschmackssinn",
    "Nicht vorhanden"
    ],
    [
    "Heiserkeit",
    "Nicht vorhanden"
    ],
    [
    "Husten",
    "Vorhanden"
    ],
    [
    "Schnupfen",
    "Vorhanden"
    ]```
18. Run Query `SELECT  n/value/value,  l/value/value FROM EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.symptom_sign_screening.v0] contains (ELEMENT l[at0005] and ELEMENT n[at0004]) where c/archetype_details/template_id/value matches { 'Corona_Anamnese'}   and e/ehr_id/value matches {'{ehr_id1}'}`
19. Check Result : ```
    [
    "Durchfall",
    "Nicht vorhanden"
    ],
    [
    "Fieber oder erhöhte Körpertemperatur",
    "Vorhanden"
    ],
    [
    "Gestörter Geruchssinn",
    "Nicht vorhanden"
    ],
    [
    "Gestörter Geschmackssinn",
    "Nicht vorhanden"
    ],
    [
    "Heiserkeit",
    "Nicht vorhanden"
    ],
    [
    "Husten",
    "Vorhanden"
    ],
    [
    "Schnupfen",
    "Vorhanden"
    ]```
20. Run Query `SELECT  n/value/value,  l/value/value FROM EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.symptom_sign_screening.v0] contains (ELEMENT l[at0005] and ELEMENT n[at0004]) where c/archetype_details/template_id/value matches { 'Corona_Anamnese'} and l/value/value = 'Vorhanden'  and e/ehr_id/value matches {'{ehr_id1}'}`
21. Check Result : ```
    [
    "Fieber oder erhöhte Körpertemperatur",
    "Vorhanden"
    ],
    [
    "Husten",
    "Vorhanden"
    ],
    [
    "Schnupfen",
    "Vorhanden"
    ]```
22. Run Query `SELECT  n/value/value,  l/value/value FROM EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.symptom_sign_screening.v0] contains (ELEMENT l[at0005] and ELEMENT n[at0004]) where c/archetype_details/template_id/value matches { 'Corona_Anamnese'} and l/value/value = 'Vorhanden' and n/value/value = 'Husten' and e/ehr_id/value matches {'{ehr_id1}'}`
23. Check Result : ```
    [
    "Husten",
    "Vorhanden"
    ]```
## Contains and where

1. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
2. Create ehr 
3. Create composition `type_repetition_conformance_ehrbase.org_where1.json` 
4. Run Query `SELECT  s1/feeder_audit/originating_system_item_ids/id, s2/feeder_audit/originating_system_item_ids/id, o/feeder_audit/originating_system_item_ids/id, v/time/value , c1/items[at0001]/value/value , l/value/value  FROM EHR e contains COMPOSITION c contains SECTION s1 contains SECTION s2 contains OBSERVATION o contains EVENT v contains  CLUSTER c1 contains CLUSTER c2 contains ELEMENT l`
5. Check Result : ```
   [
   [
   "ad_hoc_heading 1",
   "conformance_section 1",
   "observation 1",
   "2021-02-03T04:05:06",
   "cluster inter text1",
   "cluster outer text1"
   ],
   [
   "ad_hoc_heading 1",
   "conformance_section 1",
   "observation 1",
   "2021-02-03T04:05:06",
   "cluster inter text1",
   "cluster outer text2"
   ],
   [
   "ad_hoc_heading 1",
   "conformance_section 1",
   "observation 1",
   "2022-02-03T04:05:06",
   "cluster inter text2",
   "cluster outer text3"
   ],
   [
   "ad_hoc_heading 1",
   "conformance_section 1",
   "observation 1",
   "2022-02-03T04:05:06",
   "cluster inter text2",
   "cluster outer text4"
   ],
   [
   "ad_hoc_heading 1",
   "conformance_section 2",
   "observation 2",
   "2023-02-03T04:05:06",
   "cluster inter text3",
   "cluster outer text5"
   ],
   [
   "ad_hoc_heading 1",
   "conformance_section 2",
   "observation 2",
   "2023-02-03T04:05:06",
   "cluster inter text3",
   "cluster outer text6"
   ],
   [
   "ad_hoc_heading 1",
   "conformance_section 2",
   "observation 2",
   "2024-02-03T04:05:06",
   "cluster inter text4",
   "cluster outer text7"
   ],
   [
   "ad_hoc_heading 1",
   "conformance_section 2",
   "observation 2",
   "2024-02-03T04:05:06",
   "cluster inter text4",
   "cluster outer text8"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 3",
   "observation 3",
   "2021-02-03T04:05:06",
   "cluster inter text5",
   "cluster outer text9"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 3",
   "observation 3",
   "2021-02-03T04:05:06",
   "cluster inter text5",
   "cluster outer text10"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 3",
   "observation 3",
   "2022-02-03T04:05:06",
   "cluster inter text6",
   "cluster outer text11"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 3",
   "observation 3",
   "2022-02-03T04:05:06",
   "cluster inter text6",
   "cluster outer text12"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 4",
   "observation 4",
   "2023-02-03T04:05:06",
   "cluster inter text7",
   "cluster outer text13"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 4",
   "observation 4",
   "2023-02-03T04:05:06",
   "cluster inter text7",
   "cluster outer text14"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 4",
   "observation 4",
   "2024-02-03T04:05:06",
   "cluster inter text8",
   "cluster outer text15"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 4",
   "observation 4",
   "2024-02-03T04:05:06",
   "cluster inter text8",
   "cluster outer text16"
   ]
   ]```
6. Run Query `SELECT  s1/feeder_audit/originating_system_item_ids/id, s2/feeder_audit/originating_system_item_ids/id, o/feeder_audit/originating_system_item_ids/id, v/time/value , c1/items[at0001]/value/value , l/value/value  FROM EHR e contains COMPOSITION c contains SECTION s1 contains SECTION s2 contains OBSERVATION o contains EVENT v contains  CLUSTER c1 contains CLUSTER c2 contains ELEMENT l where s1/feeder_audit/originating_system_item_ids/id = 'ad_hoc_heading 2'`
7. Check Result : ```
   [
   [
   "ad_hoc_heading 2",
   "conformance_section 3",
   "observation 3",
   "2021-02-03T04:05:06",
   "cluster inter text5",
   "cluster outer text9"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 3",
   "observation 3",
   "2021-02-03T04:05:06",
   "cluster inter text5",
   "cluster outer text10"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 3",
   "observation 3",
   "2022-02-03T04:05:06",
   "cluster inter text6",
   "cluster outer text11"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 3",
   "observation 3",
   "2022-02-03T04:05:06",
   "cluster inter text6",
   "cluster outer text12"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 4",
   "observation 4",
   "2023-02-03T04:05:06",
   "cluster inter text7",
   "cluster outer text13"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 4",
   "observation 4",
   "2023-02-03T04:05:06",
   "cluster inter text7",
   "cluster outer text14"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 4",
   "observation 4",
   "2024-02-03T04:05:06",
   "cluster inter text8",
   "cluster outer text15"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 4",
   "observation 4",
   "2024-02-03T04:05:06",
   "cluster inter text8",
   "cluster outer text16"
   ]
   ]```
8. Run Query `SELECT  s1/feeder_audit/originating_system_item_ids/id, s2/feeder_audit/originating_system_item_ids/id, o/feeder_audit/originating_system_item_ids/id, v/time/value , c1/items[at0001]/value/value , l/value/value  FROM EHR e contains COMPOSITION c contains SECTION s1 contains SECTION s2 contains OBSERVATION o contains EVENT v contains  CLUSTER c1 contains CLUSTER c2 contains ELEMENT l where s1/feeder_audit/originating_system_item_ids/id = 'ad_hoc_heading 2' and s2/feeder_audit/originating_system_item_ids/id = 'conformance_section 4' and v/time/value = '2024-02-03T04:05:06'`
9. Check Result : ```
   [
   [
   "ad_hoc_heading 2",
   "conformance_section 4",
   "observation 4",
   "2024-02-03T04:05:06",
   "cluster inter text8",
   "cluster outer text15"
   ],
   [
   "ad_hoc_heading 2",
   "conformance_section 4",
   "observation 4",
   "2024-02-03T04:05:06",
   "cluster inter text8",
   "cluster outer text16"
   ]
   ]```
10. Run Query `SELECT  s1/feeder_audit/originating_system_item_ids/id, s2/feeder_audit/originating_system_item_ids/id, o/feeder_audit/originating_system_item_ids/id, v/time/value , c1/items[at0001]/value/value , l/value/value  FROM EHR e contains COMPOSITION c contains SECTION s1 contains SECTION s2 contains OBSERVATION o contains EVENT v contains  CLUSTER c1 contains CLUSTER c2 contains ELEMENT l where s1/feeder_audit/originating_system_item_ids/id = 'ad_hoc_heading 2' and s2/feeder_audit/originating_system_item_ids/id = 'conformance_section 4' and v/time/value = '2024-02-03T04:05:06' and l/value/value = 'cluster outer text16'`
11. Check Result : ```
    [
    [
    "ad_hoc_heading 2",
    "conformance_section 4",
    "observation 4",
    "2024-02-03T04:05:06",
    "cluster inter text8",
    "cluster outer text16"
    ]
    ]```