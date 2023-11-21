# WHERE [Feature List](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#WHERE)
## Simple Compare
### Compare with extracted column I  https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#WHERE
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
3. Create ehr
4. Save ehr_id as {ehr_id_1}
5. Create composition  `conformance_ehrbase.de.v0_max.json`
6. Save comp_id as {comp_id_1}
7. Create ehr
8. Save ehr_id as {ehr_id_2}
9. Create composition  `type_repetition_conformance_ehrbase.org_one_reptation.json`
10. Save comp_id as {comp_id_2}
11. Run Query "SELECT e/ehr_id/value, c/uid/value from EHR e CONTAINS COMPOSITION c WHERE {where}"

| {where}                                                           | e/ehr_id/value | c/uid/value |
|-------------------------------------------------------------------|----------------|-------------|
| e/ehr_id/value = {ehr_id_1}                                       | {ehr_id_1}     | {comp_id_1} |
| e/ehr_id/value = {ehr_id_2}                                       | {ehr_id_2}     | {comp_id_2} |
| e/ehr_id/value != {ehr_id_1}                                      | {ehr_id_2}     | {comp_id_2} |
| e/ehr_id/value != {ehr_id_2}                                      | {ehr_id_1}     | {comp_id_1} |
| c/uid/value = {comp_id_1}                                         | {ehr_id_1}     | {comp_id_1} |
| c/uid/value = {comp_id_2}                                         | {ehr_id_2}     | {comp_id_2} |
| c/uid/value != {comp_id_1}                                        | {ehr_id_2}     | {comp_id_2} |
| c/uid/value != {comp_id_2}                                        | {ehr_id_1}     | {comp_id_1} |
| c/archetype_details/template_id/value = conformance-ehrbase.de.v0 | {ehr_id_1}     | {comp_id_1} |
| c/name/value = type_repetition_conformance_ehrbase.org            | {ehr_id_2}     | {comp_id_2} |

### Compare with extracted column II  https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#WHERE
1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Run Query 'Select `SELECT o FROM COMPOSITION contains OBSERVATION o where {where}`

| {where}                                                                    | o/uid/value                                                                                                                                                                                             |
|----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| o/archetype_node_id = "openEHR-EHR-OBSERVATION.conformance_observation.v0" | Returns 4 observations with o/uid/value = ["94c0e756-e892-4985-884b-46829605a236","d4cccdfc-9c90-402f-b4bb-94e8dc4ea429","55415141-17e4-4c71-9429-aa0fe6694c83","893506a7-462b-40b8-9638-0aa3990642d9"] |
| o/name/value = "Blood pressure"                                            | Returns 1 observation with o/uid/value = "2183807d-af68-41c5-9bfe-28cd150d62f7"                                                                                                                         |

### Compare by Paths within same hierarchy level 

1. Upload `conformance-ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_known_date_type_1.json`
4. Create composition  `conformance_ehrbase.de.v0_known_date_type_2.json`
5. Run Query 'Select `SELECT {path} from EHR e CONTAINS COMPOSITION c contains EVENT_CONTEXT ec WHERE {where}`

| {path}                                        | {where}                                                  |                                                                     |
|-----------------------------------------------|----------------------------------------------------------|---------------------------------------------------------------------|
| ec/health_care_facility/external_ref/id/value | ec/health_care_facility/external_ref/id/value = '9091'   | 9091                                                                |
| ec/health_care_facility/external_ref/id/value | ec/health_care_facility/external_ref/id/value = '9092'   | 9092                                                                |
| ec/location                                   | ec/location = 'Hospital'                                 | Hospital                                                            |
| ec/location                                   | ec/location = 'microbiology lab 2'                       | microbiology lab 2                                                  |
| ec/start_time/value                           | ec/start_time = '2021-12-21T14:19:31.649613+01:00'       | 2021-12-21T14:19:31.649613+01:00                                    |
| ec/start_time/value                           | ec/start_time = '2022-12-21T14:19:31.649613+01:00'       | 2022-12-21T14:19:31.649613+01:00                                    |
| ec/start_time/value                           | ec/start_time/value = '2021-12-21T14:19:31.649613+01:00' | 2021-12-21T14:19:31.649613+01:00                                    |
| ec/start_time/value                           | ec/start_time/value = '2022-12-21T14:19:31.649613+01:00' | 2022-12-21T14:19:31.649613+01:00                                    |

### Compare by Paths over hierarchy level

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_where.json`
4. Run Query 'Select `SELECT {path} FROM COMPOSITION C CONTAINS OBSERVATION o WHERE {path} = {value} `



   | path                                                                     | {value}               | result              |
   |--------------------------------------------------------------------------|-----------------------|---------------------|
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | 'Lorem ipsum'         | "Lorem ipsum"       |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | 'Lorem ipsum 2'       | "Lorem ipsum 2"     |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude | 20                    | 20                  |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude | 25                    | 25                  |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value/value     | '2022-02-03T04:05:06' | 2022-02-03T04:05:06 |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value/value     | '2024-02-03T04:05:06' | 2024-02-03T04:05:06 |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0017]/value/value     | true                  | true                |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0017]/value/value     | false                 | false               |
   

### Compare with Array valued paths

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
5. Run Query 'Select `SELECT {path} FROM COMPOSITION C CONTAINS OBSERVATION o WHERE {where} `

   | path                                                                                                                                                                                                                                                                                                                                      | {where}                                                                                                                 | result                                            |
   |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|
   | c/context/participations/performer/name, c/context/participations/performer/external_ref/id/value                                                                                                                                                                                                                                         | c/context/participations/performer/name ='Dr. Marcus Johnson'                                                           | "Dr. Marcus Johnson,199"                          |
   | c/context/participations/performer/name, c/context/participations/performer/external_ref/id/value                                                                                                                                                                                                                                         | c/context/participations/performer/external_ref/id/value  ='200'                                                        | "Dr. Stefan Mann  200"                            |
   | c/context/participations/performer/name, c/context/participations/performer/identifiers/id                                                                                                                                                                                                                                                | c/context/participations/performer/name ='Dr. Marcus Johnson'                                                           | "Dr. Marcus Johnson,200","Dr. Marcus Johnson,201" |
   | c/context/participations/performer/name, c/context/participations/performer/identifiers/id                                                                                                                                                                                                                                                | c/context/participations/performer/identifiers/id  ='202'                                                               | "Dr. Stefan Mann,202"                             |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/lower/magnitude, o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/upper/magnitude, o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/upper/meaning/value | o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/lower/magnitude = 8         | "8,10,high"                                       |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/lower/magnitude, o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/upper/magnitude, o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/upper/meaning/value | o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/upper/magnitude = 12        | "11,12,very high"                                 |
   | o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/lower/magnitude, o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/upper/magnitude, o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/upper/meaning/value | o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/other_reference_ranges/range/upper/meaning/value ='high' | "8,10,high"                                       |

### Compare by DV_DATE

1. Upload `conformance-ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_known_date_type_1.json`
4. Create composition  `conformance_ehrbase.de.v0_known_date_type_2.json`
5. Create composition  `conformance_ehrbase.de.v0_known_date_type_3.json`
6. Create composition  `conformance_ehrbase.de.v0_known_date_type_4.json`
7. Run Query 'Select `SELECT {path}/value from EHR e CONTAINS COMPOSITION c contains EVENT_CONTEXT ec WHERE {path} {condition}`

| {path}        | {condition}                           | result                                                                                               |
|---------------|---------------------------------------|------------------------------------------------------------------------------------------------------|
| ec/start_time | = '2021-12-21T15:19:31.649613+01:00'  | 2021-12-21T15:19:31.649613+01:00                                                                     |
| ec/start_time | = '2021-12-21T15:19:31.649613+02:00'  | 2021-12-21T14:19:31.649613+01:00                                                                     |
| ec/start_time | >= '2021-12-21T15:19:31.649613+01:00' | 2021-12-21T15:19:31.649613+01:00 2022-12-21T14:19:31.649613+01:00                                    |
| ec/start_time | > '2021-12-21T15:19:31.649613+01:00'  | 2022-12-21T14:19:31.649613+01:00                                                                     |
| ec/start_time | <= '2021-12-21T15:19:31.649613+01:00' | 2021-12-21T14:19:31.649613+03:00, 2021-12-21T14:19:31.649613+01:00, 2021-12-21T15:19:31.649613+01:00 |
| ec/start_time | < '2021-12-21T15:19:31.649613+01:00'  | 2021-12-21T14:19:31.649613+03:00, 2021-12-21T14:19:31.649613+01:00                                   |
| ec/start_time | != '2021-12-21T15:19:31.649613+01:00' | 2021-12-21T14:19:31.649613+03:00, 2021-12-21T14:19:31.649613+01:00, 2022-12-21T14:19:31.649613+01:00 |




### Compare by DvOrdered

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v2.json`
4. Run Query 'Select `SELECT {path}{spath} FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0] WHERE {path} {condition}`

| path                                                                     | spath      | condition               | result              |
|--------------------------------------------------------------------------|------------|-------------------------|---------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value           | /magnitude | = 82.0                  | 82.0                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value           | /magnitude | > 82.0                  | empty result        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value           | /magnitude | < 82.0                  | 22.0                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude |            | = 82.0                  | 82.0                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude |            | > 82.0                  | empty result        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude |            | < 82.0                  | 22.0                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value           | /numerator | =  14                   | 42                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value           | /numerator | >  14                   | 40                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value/numerator |            | = 40                    | 40                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value/numerator |            | > 40                    | 42                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/magnitude |            | = 50                    | 50                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/magnitude |            | > 50                    | 400                 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value           | /magnitude | = 50                    | 50                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value           | /magnitude | > 50                    | 400                 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value/value     |            | = '2022-03-03T04:05:06' | 2022-03-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value/value     |            | > '2022-03-03T04:05:06' | 2023-02-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value           | /value     | = '2022-03-03T04:05:06' | 2022-03-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value           | /value     | > '2022-03-03T04:05:06' | 2023-02-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value/value     |            | = '04:06:06'            | 04:06:06            |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value/value     |            | > '04:06:06'            | 05:05:06            |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value           | /value     | = '04:06:06'            | 04:06:06            |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value           | /value     | > '04:06:06'            | 05:05:06            |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value/value     |            | = '2022-03-03'          | 2022-03-03          |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value/value     |            | > '2022-03-03'          | 2023-02-03          |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value           | /value     | = '2022-03-03'          | 2022-03-03          |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value           | /value     | > '2022-03-03'          | 2023-02-03          |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value/value     |            | = 1                     | 1,1                 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value/value     |            | > 1                     | 2                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value           | /value     | = 1                     | 1,1                 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value           | /value     | > 1                     | 2                   |


### Compare BY unknown type

1. Upload `choice_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `choice_ehrbase.de.v0.json`
4. Run Query '
   Select `SELECT o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value,o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude   FROM OBSERVATION o [openEHR-EHR-OBSERVATION.choice.v0] WHERE {path} {condition}`

| path                                                                     | condition     | result                                                                                                                                                                                                                                   |
|--------------------------------------------------------------------------|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value           | =  '04:05:06' | {'04:05:06',NULL}                                                                                                                                                                                                                        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value           | <  '04:06:06' | {'04:05:06',NULL}                                                                                                                                                                                                                        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value           | =  '10.0'     | empty                                                                                                                                                                                                                                    |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value           | <  '2'        | empty                                                                                                                                                                                                                                    |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value           | =  2          | {2,NULL}, {2.0,NULL}                                                                                                                                                                                                                     |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value           | <  10         | {NULL,2},{NULL,2.0} ,{NULL,3}                                                                                                                                                                                                            |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value           | =  false      | empty                                                                                                                                                                                                                                    |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value           | <  false      | empty                                                                                                                                                                                                                                    |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | =  '04:05:06' | {'04:05:06',NULL}                                                                                                                                                                                                                        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | <  '04:06:06' | {'04:05:06',NULL}                                                                                                                                                                                                                        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | =  '10.0'     | {'10.0',NULL}                                                                                                                                                                                                                            |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | <  '2'        | {'04:05:06',NULL},{'04:06:06',NULL}, {'10.0',NULL}                                                                                                                                                                                       |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | =  2          | empty set                                                                                                                                                                                                                                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | <  10         | {'04:05:06',NULL},{'04:06:06',NULL}, {'10.0',NULL},{'2',NULL},{'2022-03-03T04:05:06',NULL}, {'2022-03-03T04:06:06',NULL},{'2022-03-04',NULL},{'2022-03-04T04:05:06',NULL},{'2022-04-03T04:05:06',NULL}                                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | =  false      | {false,NULL}                                                                                                                                                                                                                             |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | <  false      | {'a',null},{'b',null},{'04:05:06',NULL},{'04:06:06',NULL}, {'10.0',NULL},{'2',NULL},{'2022-03-03T04:05:06',NULL}, {'2022-03-03T04:06:06',NULL},{'2022-03-04',NULL},{'2022-03-04T04:05:06',NULL},{'2022-04-03T04:05:06',NULL},{true,NULL} |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude | =  '04:05:06' | empty set                                                                                                                                                                                                                                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude | <  '04:06:06' | empty set                                                                                                                                                                                                                                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude | =  '10.0'     | empty set                                                                                                                                                                                                                                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude | <  '2'        | empty set                                                                                                                                                                                                                                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude | =  2          | {2,NULL}, {2.0,NULL}                                                                                                                                                                                                                     |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude | <  10         | {NULL,2},{NULL,2.0}, {NULL,3}                                                                                                                                                                                                            |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude | =  false      | empty set                                                                                                                                                                                                                                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/magnitude | <  false      | {NULL,2},{NULL,2.0},{NULL,10.0}, {NULL,10}, {NULL,3}                                                                                                                                                                                     |



## Exists
### Compare with Null

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v2.json`
4. Run Query 'Select `SELECT {path}{spath} FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0] WHERE {condition} {path} `

| path                                                                     | spath      | condition  | result     |
|--------------------------------------------------------------------------|------------|------------|------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value           | /magnitude | EXISTS     | 22.0, 80.2 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude |            | EXISTS     | 22.0, 80.2 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value           | /magnitude | NOT EXISTS | NULL       |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude |            | NOT EXISTS | NULL       |

## Matches [Feature List](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#WHERE)
### Matches extracted column
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Save ehr_id as {ehr_id_1}
4. Create composition  `conformance_ehrbase.de.v0_max.json`
5. Save comp_id as {comp_id_1}
6. Create ehr
7. Save ehr_id as {ehr_id_2}
8. Create composition  `conformance_ehrbase.de.v0_max.json`
9. Save comp_id as {comp_id_2}
10. Create ehr
11. Save ehr_id as {ehr_id_3}
12. Create composition  `conformance_ehrbase.de.v0_max.json`
13. Save comp_id as {comp_id_4}
14. Run Query "SELECT e/ehr_id/value, c/uid/value from EHR e CONTAINS COMPOSITION c WHERE {where}"


| {where}                                         | e/ehr_id/value,  c/uid/value                        |
|-------------------------------------------------|-----------------------------------------------------|
| e/ehr_id/value matches {{ehr_id_1}, {ehr_id_2}} | ["{ehr_id_1},{comp_id_1}","{ehr_id_2},{comp_id_2}"] |
| e/ehr_id/value matches {{ehr_id_1}, {ehr_id_3}} | ["{ehr_id_1},{comp_id_1}","{ehr_id_3},{comp_id_3}"] |

## like [Feature List](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#WHERE)
### like with extracted column 
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
3. Create ehr
4. Save ehr_id as {ehr_id_1}
5. Create composition  `conformance_ehrbase.de.v0_max.json`
6. Save comp_id as {comp_id_1}
7. Create ehr
8. Save ehr_id as {ehr_id_2}
9. Create composition  `type_repetition_conformance_ehrbase.org_one_reptation.json`
10. Save comp_id as {comp_id_2}
11. Run Query "SELECT c/name/value, c/uid/value from EHR e CONTAINS COMPOSITION c WHERE c/name/value {like}"

| {like}                                  | c/name/value                                                        | c/uid/value               |
|-----------------------------------------|---------------------------------------------------------------------|---------------------------|
| type_repetition_conformance_ehrbase.org | type_repetition_conformance_ehrbase.org                             | {comp_id_2}               |
| type?repetition?conformance?ehrbase*    | type_repetition_conformance_ehrbase.org                             | {comp_id_2}               |
| \*ehrbase\*                             | [conformance-ehrbase.de.v0,type_repetition_conformance_ehrbase.org] | [{comp_id_1},{comp_id_2}] |

## like [Feature List](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#WHERE)

### like with extracted column 
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
3. Create ehr
4. Save ehr_id as {ehr_id_1}
5. Create composition  `conformance_ehrbase.de.v0_max.json`
6. Save comp_id as {comp_id_1}
7. Create ehr
8. Save ehr_id as {ehr_id_2}
9. Create composition  `type_repetition_conformance_ehrbase.org_one_reptation.json`
10. Save comp_id as {comp_id_2}
11. Run Query "SELECT c/name/value, c/uid/value from EHR e CONTAINS COMPOSITION c WHERE c/name/value LIKE {like}"

| {like}                                  | c/name/value                                                        | c/uid/value               |
|-----------------------------------------|---------------------------------------------------------------------|---------------------------|
| type_repetition_conformance_ehrbase.org | type_repetition_conformance_ehrbase.org                             | {comp_id_2}               |
| type?repetition?conformance?ehrbase*    | type_repetition_conformance_ehrbase.org                             | {comp_id_2}               |
| \*ehrbase*                              | [conformance-ehrbase.de.v0,type_repetition_conformance_ehrbase.org] | [{comp_id_1},{comp_id_2}] |


### like with extracted column II
1. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
2. Create ehr
3. Create composition  `type_repetition_conformance_ehrbase.org_like.json`
4. Run Query "SELECT s/name/value from EHR e CONTAINS COMPOSITION c CONTAINS SECTION s WHERE s/name/value LIKE {like}"

| {like}     | c/name/value  |
|------------|---------------|
| Name%      | []            |
| Name%_     | Name%_        |
| *%_        | Name%_        |
| Name*      | Name%_,Name*? |
| Name\\*    | []            |
| Name\\*\\? | Name*?        |
| \*\\*\\?   | Name*?        |



## Boolean Operations
### Single  `AND` / `OR` [Feature List](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#AND-%2F-OR.1)
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Save ehr_id as {ehr_id_1}
4. Create composition  `conformance_ehrbase.de.v0_max.json`
5. Save comp_id as {comp_id_1}
6. Create composition  `conformance_ehrbase.de.v0_max.json`
7. Save comp_id as {comp_id_2}
8. Create ehr
9. Save ehr_id as {ehr_id_2}
10. Run Query "SELECT e/ehr_id/value, c/uid/value from EHR e CONTAINS COMPOSITION c WHERE {where}"

| {where}                                                     | e/ehr_id/value,  c/uid/value                        |
|-------------------------------------------------------------|-----------------------------------------------------|
| e/ehr_id/value = {ehr_id_1} and  c/uid/value = {comp_id_1}  | ["{ehr_id_1},{comp_id_1}"]                          |
| e/ehr_id/value = {ehr_id_1} or  c/uid/value = {comp_id_1}   | ["{ehr_id_1},{comp_id_1}","{ehr_id_1},{comp_id_2}"] |
| c/uid/value = {comp_id_1} or  c/uid/value = {comp_id_2}     | ["{ehr_id_1},{comp_id_1}","{ehr_id_1},{comp_id_2}"] |
| c/uid/value = {comp_id_1} and  c/uid/value = {comp_id_2}    | []                                                  |
| e/ehr_id/value != {ehr_id_1} and  c/uid/value = {comp_id_1} | []                                                  |
| e/ehr_id/value != {ehr_id_2} and  c/uid/value = {comp_id_1} | ["{ehr_id_1},{comp_id_1}"]                          |

### Chaining  `AND` / `OR`
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Save ehr_id as {ehr_id_1}
4. Create composition  `conformance_ehrbase.de.v0_max.json`
5. Save comp_id as {comp_id_1}
6. Create composition  `conformance_ehrbase.de.v0_max.json`
7. Save comp_id as {comp_id_2}
8. Create ehr
9. Save ehr_id as {ehr_id_2}
10. Create composition  `conformance_ehrbase.de.v0_max.json`
11. Save comp_id as {comp_id_3}
12. Create composition  `conformance_ehrbase.de.v0_max.json`
13. Save comp_id as {comp_id_4}
14. Run Query "SELECT e/ehr_id/value, c/uid/value from EHR e CONTAINS COMPOSITION c WHERE {where}"

| {where}                                                                                                                     | e/ehr_id/value,  c/uid/value                        |
|-----------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| (e/ehr_id/value = {ehr_id_1} and  c/uid/value = {comp_id_1}) or c/uid/value = {comp_id_3}                                   | ["{ehr_id_1},{comp_id_1}","{ehr_id_2},{comp_id_3}"] |
| e/ehr_id/value = {ehr_id_1} and  (c/uid/value = {comp_id_1} or c/uid/value = {comp_id_3})                                   | ["{ehr_id_1},{comp_id_1}"                           |
| e/ehr_id/value = {ehr_id_1} and  c/uid/value = {comp_id_1} or c/uid/value = {comp_id_3}                                     | ["{ehr_id_1},{comp_id_1}","{ehr_id_2},{comp_id_3}"] |
| (e/ehr_id/value = {ehr_id_1} or e/ehr_id/value = {ehr_id_2}) and  (c/uid/value = {comp_id_1} or c/uid/value = {comp_id_3})  | ["{ehr_id_1},{comp_id_1}","{ehr_id_2},{comp_id_3}"] |
| (e/ehr_id/value = {ehr_id_1} and c/uid/value = {comp_id_1}) or  (e/ehr_id/value = {ehr_id_2} and c/uid/value = {comp_id_4}) | ["{ehr_id_1},{comp_id_1}","{ehr_id_2},{comp_id_4}"] |



