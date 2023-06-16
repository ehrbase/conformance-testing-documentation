# SELECT

## AS  [General keywords A-D](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#AS(%5BinlineCard%5D-))

### AS in result

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Run Query 'Select `SELECT c AS full FROM COMPOSITIONS c`
5. Check that the column c is named full in the result

## DISTINCT  [General keywords A-D](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#AS(%5BinlineCard%5D-))

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create 2 x composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Run Query 'Select `SELECT e/ehr_id/value AS full FROM EHR e contains COMPOSITIONS C`
5. Check Result 2 Rows
6. Run Query 'Select `SELECT DISTINCT e/ehr_id/value AS full FROM EHR e contains COMPOSITIONS C`
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

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT i/narrative/value FROM INSTRUCTION i`
5. Returns 1 Row "Human readable instruction narrative"

### Paths within same hierarchy level III

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT c/start_time/value FROM EVENT_CONTEXT c`
5. Returns 1 Row "2021-12-21T14:19:31.649613+01:00"

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
