# SELECT

## AS  [General keywords A-D](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#AS(%5BinlineCard%5D-))

### AS in result

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Run Query 'Select `SELECT c AS full FROM COMPOSITION c`
5. Check that the column c is named full in the result

## DISTINCT  [General keywords A-D](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#AS(%5BinlineCard%5D-))

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create 2 x composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Run Query 'Select `SELECT e/ehr_id/value AS full FROM EHR e contains COMPOSITION C`
5. Check Result 2 Rows
6. Run Query 'Select `SELECT DISTINCT e/ehr_id/value AS full FROM EHR e contains COMPOSITION C`
7. Check Result 1 Rows

## Identified Path [Feature A-D](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#Identified-Path-(%5BinlineCard%5D))

### Path to target Object

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Run Query 'Select `SELECT c FROM COMPOSITION c`
5. Check return is the whole composition
6. Run Query 'Select `SELECT c/uid FROM COMPOSITION c`
7. Check return is a json of type _OBJECT_VERSION_ID
8. Run Query 'Select `SELECT c/uid/value FROM COMPOSITION c`
9. Check return is a String with the uid

### Paths within same hierarchy level I

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Run Query `Select p/time/value FROM POINT_EVENT p`
5. 4 Rows :

   | p/time/value        |
   |---------------------|
   | 2022-02-03T04:05:06 |
   | 2023-02-03T04:05:06 |
   | 2024-02-03T04:05:06 |
   | 2025-02-03T04:05:06 |

### Paths within same hierarchy level II

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT i/narrative/value FROM INSTRUCTION i`
5. Returns 1 Row "Human readable instruction narrative"

### Paths within same hierarchy level III

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT c/start_time/value FROM EVENT_CONTEXT c`
5. Returns 1 Row "2021-12-21T14:19:31.649613+01:00"

### Subobjects within locatable

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT c/setting FROM EVENT_CONTEXT c`
5. Returns 1 Row '''{
   "_type": "DV_CODED_TEXT",
   "value": "other care",
   "defining_code": {
   "_type": "CODE_PHRASE",
   "terminology_id": {
   "_type": "TERMINOLOGY_ID",
   "value": "openehr"
   }'''

### Path to new locatable

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query '
   Select `SELECT c/content[openEHR-EHR-SECTION.conformance_section.v0]/items[openEHR-EHR-ACTION.conformance_action_.v0] FROM COMPOSITION c`
5. Returns 1 Row json with type "ACTION"

### Path from Entry to Data value

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v3.json`
4. Run Query 'Select `SELECT {path} FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0] `

| path                                                                        | result                                                                                                                                 |
|-----------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value        | "Lorem ipsum", "Lorem ipsum2", "Lorem ipsum3"                                                                                          |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0005]/value/value        | "term1", "term1", "term1"                                                                                                              |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/units        | NULL, "mm", "mm"                                                                                                                       |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude    | NULL, 22.0, 80.2                                                                                                                       |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/null_flavour/value | "unknown", NULL, NULL                                                                                                                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value/numerator    | 42.0,40.0,20.0                                                                                                                         |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value/denominator  | 3.0,2.0,2.0                                                                                                                            |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/magnitude    | 42,400,51                                                                                                                              |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value/value        | 2022-02-03T04:05:06, 2023-02-03T04:05:06,2022-02-03T04:05:06                                                                           |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value/value        | 04:05:06,05:05:06,04:05:06                                                                                                             |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value/value        | 2022-02-03,2023-02-03,2022-02-03                                                                                                       |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value/value        | 1,2,1                                                                                                                                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0017]/value/value        | true,false,true                                                                                                                        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0018]/value/value        | "PT0S,"PT10S","PT6M40S"                                                                                                                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0019]/value/id           | dev/null,dev/null2,dev/null3                                                                                                           |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0025]/value/value        | "ehr:/.","ehr:/.","ehr:/."                                                                                                             |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0026]/value/size         | 3 x 504903212                                                                                                                          |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0027]/value/value        | 3 x "<!DOCTYPE html><html lang=\"en\"><head><meta charset=\"UTF-8\"><title>Hello World</title></head><body>Hello World!</body></html>" |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0028]/value/value        | 3 x " "https://www.example.com/sample""                                                                                                |

### Path from Entry to Data value in Cluster

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v3.json`
4. Run Query '
   Select `SELECT o/data[at0001]/events[at0002]/data[at0003]/items[openEHR-EHR-CLUSTER.conformance_cluster.v0]/{path} FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0] `
5. Run Query '
   Select `SELECT c/{path} FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0] contains Cluster c [openEHR-EHR-CLUSTER.conformance_cluster.v0]`
6. Check that both have the same result

| path                                        | result                                        |
|:--------------------------------------------|-----------------------------------------------|
| items[at0003]/value/value                   | "Lorem ipsum", "Lorem ipsum2" , NULL          |
| items[at0005]/value/value                   | "Lorem ipsum", "Lorem ipsum2", "Lorem ipsum3" |
| feeder_audit/originating_system_item_ids/id | json with type FEEDER_AUDIT , NULL, NULL      |
| feeder_audit/originating_system_item_ids/id | "id1","id2", NULL, NULL                       |

### Array valued paths

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_array_valued.json`
4. Run Query 'Select `SELECT {path} FROM COMPOSITION C CONTAINS OBSERVATION o  `

   | path                                                                                                                                                                                                                                                                                                                                      | result                                                                                         |
   |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
   | c/context/participations/performer/name, c/context/participations/performer/external_ref/id/value                                                                                                                                                                                                                                         | "Dr. Marcus Johnson,199", "Dr. Stefan Mann,200"                                                |
   | c/context/participations/performer/name, c/context/participations/performer/identifiers/id                                                                                                                                                                                                                                                | "Dr. Marcus Johnson,200","Dr. Marcus Johnson,201", "Dr. Stefan Mann,202","Dr. Stefan Mann,203" |
   | c/feeder_audit/original_content/value, c/feeder_audit/feeder_system_item_ids/id, c/feeder_audit/feeder_system_item_ids/type, c/feeder_audit/feeder_system_item_ids/issuer                                                                                                                                                                 | "Hello world!,id1,PERSON,issuer1" ,"Hello world!,id2,PERSON,issuer2"                           |
   | o/subject/identifiers/id                                                                                                                                                                                                                                                                                                                  | "200", "123"                                                                                   |
   | o/other_participations/performer/name,  o/other_participations/performer/external_ref/id                                                                                                                                                                                                                                                  | "Dr. Marcus Johnson,199", "Lara Markham ,198"                                                  |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/mappings/match, o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/mappings/target/code_string                                                                                                                                                                 | "=,21794005", ">,21794007"                                                                     |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/lower/magnitude, o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/upper/magnitude, o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/upper/meaning/value | "8,10,high", "11,12,very high"                                                                 |

## Primitives

### Primitives in simple select

1. Create EHR
2. Run Query 'SELECT {primitive} from ehr'
3. Check result is equal to {primitive}

| {primitive}                        |
|------------------------------------|
| "A"                                |
| 1                                  |
| 1.1                                |
| 3e102                              |
| 7.51e10−9                          |
| TRUE                               |
| "2021-12-21T14:19:31.649613+01:00" |
| NULL                               |

### multi selects

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Save ehr id as {ehr_id}
4. Create 2 times composition `aql-conformance-ehrbase.org.v0_contains.json`
5. Save composition id as {c_uid1} and {c_uid2}
6. Run Query '
   Select `SELECT e/ehr_id/value, c/uid/value, o/uid/value, p/time/value FROM EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o CONTAINS POINT_EVENT p`
7. in any order 8 Rows

| e/ehr_id/value | c/uid/value | o/uid/value                          | p/time/value        |
|----------------|-------------|--------------------------------------|---------------------|
| {ehr_id}       | {c_uid1}    | d86702c2-15db-4d85-be51-af335cf43123 | 2022-02-03T04:05:06 |
| {ehr_id}       | {c_uid1}    | d86702c2-15db-4d85-be51-af335cf43123 | 2023-02-03T04:05:06 |
| {ehr_id}       | {c_uid1}    | 38065052-af1f-4b0b-b03d-9bda140a6edf | 2024-02-03T04:05:06 |
| {ehr_id}       | {c_uid1}    | 38065052-af1f-4b0b-b03d-9bda140a6edf | 2025-02-03T04:05:06 |
| {ehr_id}       | {c_uid2}    | d86702c2-15db-4d85-be51-af335cf43123 | 2022-02-03T04:05:06 |
| {ehr_id}       | {c_uid2}    | d86702c2-15db-4d85-be51-af335cf43123 | 2023-02-03T04:05:06 |
| {ehr_id}       | {c_uid2}    | 38065052-af1f-4b0b-b03d-9bda140a6edf | 2024-02-03T04:05:06 |
| {ehr_id}       | {c_uid2}    | 38065052-af1f-4b0b-b03d-9bda140a6edf | 2025-02-03T04:05:06 |

## Drill down along paths

### Drill down EVENT-CONTEXT

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT {path} FROM EVENT_CONTEXT c`

| path                                         | result                                                                                                                                                                                                           |
|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| c                                            | one row with type EVENT_CONTEXT                                                                                                                                                                                  |
| c/start_time                                 | one row with '''{"_type": "DV_DATE_TIME","value": "2021-12-21T14:19:31.649613+01:00"}'''                                                                                                                         |
| c/start_time/value                           | one row with   "2021-12-21T14:19:31.649613+01:00"                                                                                                                                                                |
| c/end_time                                   | one row with '''{"_type": "DV_DATE_TIME","value": "2021-12-21T14:19:31.649613+01:00"}'''                                                                                                                         |
| c/end_time/value                             | one row with   "2021-12-21T14:19:31.649613+01:00"                                                                                                                                                                |
| c/location                                   | one row with   "microbiology lab 2"                                                                                                                                                                              |
| c/setting                                    | one row with   '{ "_type": "DV_CODED_TEXT", "value": "other care", "defining_code": {  "_type": "CODE_PHRASE",  "terminology_id": {  "_type": "TERMINOLOGY_ID",  "value": "openehr"  },  "code_string": "238" }' |
| c/setting/value                              | one row with    "other care"                                                                                                                                                                                     |
| c/setting/code_string                        | one row with    "238"                                                                                                                                                                                            |
| c/setting/defining_code                      | one row with   ' {  "_type": "CODE_PHRASE",  "terminology_id": {  "_type": "TERMINOLOGY_ID",  "value": "openehr"  }'                                                                                             |
| c/setting/defining_code/terminology_id       | one row with   ' {  "_type": "TERMINOLOGY_ID",  "value": "openehr"  }'                                                                                                                                           |
| c/setting/defining_code/terminology_id/value | one row with    "openehr"                                                                                                                                                                                        |

### Drill down OBSERVATION

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT {path} FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0] `

| path                                                                                       | result                                                                                                                                                                                                                  |
|--------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| o                                                                                          | one row with type OBSERVATION                                                                                                                                                                                           |
| o/other_participations                                                                     | 2  rows with type PARTICIPATION                                                                                                                                                                                         |
| o/other_participations/function                                                            | 2  rows with type DV_TEXT                                                                                                                                                                                               |
| o/other_participations/function/value                                                      | 2  rows with requester, performer                                                                                                                                                                                       |
| o/links                                                                                    | 1  row with { "type": { "_type": "DV_TEXT", "value": "problem" }, "target": { "value": "ehr://ehr.network/347a5490-55ee-4da9-b91a-9bba710f730e" }, "meaning": { "_type": "DV_TEXT", "value": "problem related note" } } |
| o/links/type                                                                               | 1  row with  { "_type": "DV_TEXT", "value": "problem" }                                                                                                                                                                 |
| o/links/type/value                                                                         | 1  row with  "problem"                                                                                                                                                                                                  |
| o/data[at0001]                                                                             | 1  row with  name/value = "History"                                                                                                                                                                                     |
| o/data[at0001]/origin/value                                                                | 1  row with  2022-02-03T04:05:06                                                                                                                                                                                        |
| o/data[at0001]/events[at0002]                                                              | 3  row with type =   2x INTERVAL_EVENT, 1 x POINT_EVENT                                                                                                                                                                 |
| o/data[at0001]/events[at0002]                                                              | 3  row with type =   2x INTERVAL_EVENT, 1 x POINT_EVENT                                                                                                                                                                 |
| o/data[at0001]/events[at0002]/time/value                                                   | 3  row with  2022-02-03T04:05:06                                                                                                                                                                                        |
| o/data[at0001]/events[at0002]/width                                                        | 3  row with  2 X times  _type= DV_DURATION and one null                                                                                                                                                                 |
| o/data[at0001]/events[at0002]/width/value                                                  | 3  row with  P30D, NULL,  PT42H                                                                                                                                                                                         |
| o/data[at0001]/events[at0002]/sample_count                                                 | 3  row with  5, NULL,  NULL                                                                                                                                                                                             |
| o/data[at0001]/events[at0002]/data[at0003]                                                 | 3  row with  type ITEM_TREE                                                                                                                                                                                             |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]                                   | 3  row with  type Element                                                                                                                                                                                               |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value                             | 3  row with  type DV_TEXT exactly one with mapping != null                                                                                                                                                              |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value                       | "Lorem ipsum", "Lorem ipsum2", "Lorem ipsum3"                                                                                                                                                                           |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/mappings                    | 4  row with  with 2 not null json and 2 null                                                                                                                                                                            |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/mappings/target/code_string | 4  row with  with 21794005,21794000, null, null                                                                                                                                                                         |

















