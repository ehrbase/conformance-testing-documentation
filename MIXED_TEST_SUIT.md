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