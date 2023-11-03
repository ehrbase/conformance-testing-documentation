# ORDER BY

## Single Order BY

### Order BY on extracted columns I

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
3. Create ehr
5. Create composition  `aql-conformance-ehrbase.org.v0_contains.json`
6. Save comp_id as {comp_id_1}
9. Create composition  `type_repetition_conformance_ehrbase.org_one_reptation.json`
10. Save comp_id as {comp_id_2}
11. Run Query "SELECT c/name/value from EHR e CONTAINS COMPOSITION c ORDER BY {order}"

| {order}                 | result in order                                                            |
|-------------------------|----------------------------------------------------------------------------|
| c/name/value            | "aql-conformance-ehrbase.org.v0, type_repetition_conformance_ehrbase.org   |
| c/name/value ASC        | aql-conformance-ehrbase.org.v0, type_repetition_conformance_ehrbase.org    |
| c/name/value ASCENDING  | aql-conformance-ehrbase.org.v0, type_repetition_conformance_ehrbase.org    |
| c/name/value DESC       | type_repetition_conformance_ehrbase.org,    aql-conformance-ehrbase.org.v0 |
| c/name/value DESCENDING | type_repetition_conformance_ehrbase.org,    aql-conformance-ehrbase.org.v0 |

### Order BY on extracted columns II

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
3. Create ehr
4. Create composition  `aql-conformance-ehrbase.org.v0_contains.json`
5. Save comp_id as {comp_id_1}
6. Create composition  `type_repetition_conformance_ehrbase.org_one_reptation.json`
7. Save comp_id as {comp_id_2}
8. Run Query "SELECT o/name/value from EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o ORDER BY {order}"

| {order}           | result in order                                    |
|-------------------|----------------------------------------------------|
| o/name/value ASC  | 2 x  Blood pressure, 5 x  Conformance Observation  |
| o/name/value DESC | 5 x  Conformance Observation, 2 x  Blood pressure, |

### Order BY on extracted columns III

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
3. Create ehr with {ehr_id1} = 5840970c-feb7-47d8-afca-12bfc8486b81
4. Create composition  `aql-conformance-ehrbase.org.v0_contains.json` with comp_id set to {comp_id_1} =
   9b9725a1-347a-40dd-8c68-e78938ec0f19::local.ehrbase.org::1
5. Create ehr with {ehr_id2} = cc779563-6505-4f87-9e6b-c9ba5dc887fb
6. Create composition  `type_repetition_conformance_ehrbase.org_one_reptation.json` with comp_id set to {comp_id_2} =
   a9ac5dd6-6cd6-4adb-9eb5-586930ae13e9::local.ehrbase.org::1
7. Run Query "SELECT {path} from EHR e CONTAINS COMPOSITION c ORDER BY {path} {order}"

| {path}                                | {order} | result in order                                                                                                        |
|---------------------------------------|---------|------------------------------------------------------------------------------------------------------------------------|
| e/ehr_id/value                        | ASC     | 5840970c-feb7-47d8-afca-12bfc8486b81, cc779563-6505-4f87-9e6b-c9ba5dc887fb                                             |
| e/ehr_id/value                        | DESC    | cc779563-6505-4f87-9e6b-c9ba5dc887fb, 5840970c-feb7-47d8-afca-12bfc8486b81                                             |
| c/archetype_details/template_id/value | ASC     | aql-conformance-ehrbase.org.v0, type_repetition_conformance_ehrbase.org                                                |
| c/archetype_details/template_id/value | DESC    | type_repetition_conformance_ehrbase.org, aql-conformance-ehrbase.org.v0                                                |
| c/uid/value                           | ASC     | 9b9725a1-347a-40dd-8c68-e78938ec0f19::local.ehrbase.org::1, a9ac5dd6-6cd6-4adb-9eb5-586930ae13e9::local.ehrbase.org::1 |
| c/uid/value                           | DESC    | a9ac5dd6-6cd6-4adb-9eb5-586930ae13e9::local.ehrbase.org::1, 9b9725a1-347a-40dd-8c68-e78938ec0f19::local.ehrbase.org::1 |

### Order BY on extracted columns IV

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
3. Create ehr with {ehr_id1} = 5840970c-feb7-47d8-afca-12bfc8486b81
4. Create composition  `aql-conformance-ehrbase.org.v0_contains.json` with comp_id set to {comp_id_1} =
   9b9725a1-347a-40dd-8c68-e78938ec0f19::local.ehrbase.org::1
5. Create ehr with {ehr_id2} = cc779563-6505-4f87-9e6b-c9ba5dc887fb
6. Create composition  `type_repetition_conformance_ehrbase.org_one_reptation.json` with comp_id set to {comp_id_2} =
   a9ac5dd6-6cd6-4adb-9eb5-586930ae13e9::local.ehrbase.org::1
7. Run Query "SELECT {path} from EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o ORDER BY {path} {order}"

| {path}              | {order} | result in order                                                                                        |
|---------------------|---------|--------------------------------------------------------------------------------------------------------|
| o/archetype_node_id | ASC     | 2 x openEHR-EHR-OBSERVATION.blood_pressure.v2 , 5 x openEHR-EHR-OBSERVATION.conformance_observation.v0 |
| o/archetype_node_id | DESC    | 5 x openEHR-EHR-OBSERVATION.conformance_observation.v0, 2 x openEHR-EHR-OBSERVATION.blood_pressure.v2  |

### ORDER by Paths within same hierarchy level I

1. Upload `conformance-ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_known_date_type_1.json`
4. Create composition  `conformance_ehrbase.de.v0_known_date_type_2.json`
5. Run Query 'Select `SELECT {path} from EHR e CONTAINS COMPOSITION c  ORDER BY {path} {order}`

| {path}          | {order} | result in order            |
|-----------------|---------|----------------------------|
| c/composer/name | ASC     | Anton Blake , Silvia Blake |
| c/composer/name | DESC    | Silvia Blake,  Anton Blake |

### ORDER by Paths within same hierarchy level II

1. Upload `conformance-ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_known_date_type_1.json`
4. Create composition  `conformance_ehrbase.de.v0_known_date_type_2.json`
5. Run Query 'Select `SELECT {path} from EHR e CONTAINS COMPOSITION c contains EVENT_CONTEXT ec ORDER BY {path} {order}`

| {path}                                        | {order} | result in order                                                     |
|-----------------------------------------------|---------|---------------------------------------------------------------------|
| ec/health_care_facility/external_ref/id/value | ASC     | 9091, 9092                                                          |
| ec/health_care_facility/external_ref/id/value | DESC    | 9092, 9091                                                          |
| ec/location                                   | ASC     | Hospital ,microbiology lab 2                                        |
| ec/location                                   | DESC    | microbiology lab 2, Hospital                                        |
| ec/start_time                                 | ASC     | 2021-12-21T14:19:31.649613+01:00, 2022-12-21T14:19:31.649613+01:00  |
| ec/start_time                                 | DESC    | 2022-12-21T14:19:31.649613+01:00,  2021-12-21T14:19:31.649613+01:00 |
| ec/start_time/value                           | ASC     | 2021-12-21T14:19:31.649613+01:00, 2022-12-21T14:19:31.649613+01:00  |
| ec/start_time/value                           | DESC    | 2022-12-21T14:19:31.649613+01:00,  2021-12-21T14:19:31.649613+01:00 |



### ORDER by Paths within same hierarchy level III

1. Upload `conformance-ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_known_date_type_1.json`
4. Create composition  `conformance_ehrbase.de.v0_known_date_type_2.json`
5. Run Query '
   Select `SELECT {path} from EHR e CONTAINS COMPOSITION c contains EVENT_CONTEXT ec contains INTERVAL_EVENT i ORDER BY {path} {order}`

| {path}         | {order} | result in order |
|----------------|---------|-----------------|
| i/sample_count | ASC     | 5, 10           |
| i/sample_count | DESC    | 10, 5           |
| i/width        | ASC     | P40D, P30M      |
| i/width        | DESC    | P30M, P40D      |
| i/width/value  | ASC     | P30M, P40D      |
| i/width/value  | DESC    | P40D, P30M      |

### ORDER by  DV_DATE

1. Upload `conformance-ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_known_date_type_1.json`
4. Create composition  `conformance_ehrbase.de.v0_known_date_type_2.json`
5. Create composition  `conformance_ehrbase.de.v0_known_date_type_3.json`
6. Create composition  `conformance_ehrbase.de.v0_known_date_type_4.json`
5. Run Query 'Select `SELECT {path}/value from EHR e CONTAINS COMPOSITION c contains EVENT_CONTEXT ec ORDER BY {path} {order}`

| {path}                                        | {order} | result in order                                                                                                                       |
|-----------------------------------------------|---------|---------------------------------------------------------------------------------------------------------------------------------------|
| ec/start_time                                 | ASC     | 2021-12-21T14:19:31.649613+01:00,2021-12-21T15:19:31.649613+01:00,2021-12-21T14:19:31.649613+03:00, 2022-12-21T14:19:31.649613+01:00  |
| ec/start_time                                 | DESC    | 2022-12-21T14:19:31.649613+01:00,2021-12-21T14:19:31.649613+03:00 ,2021-12-21T15:19:31.649613+01:00 ,2021-12-21T14:19:31.649613+01:00 |


### ORDER BY Path from Entry to DvOrdered

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v2.json`
4. Run Query '
   Select `SELECT {path}{spath} FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0] ORDER BY {path} {order}`

| path                                                                     | spath      | order | result in Order                                              |
|--------------------------------------------------------------------------|------------|-------|--------------------------------------------------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value           | /magnitude | ASC   | In order 22.0, 82.0 , NULL                                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value           | /magnitude | DESC  | In order  NULL,82.0 , 22.0                                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude |            | ASC   | In order 22.0, 82.0 , NULL                                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude |            | DESC  | In order  NULL,82.0 , 22.0                                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value           | /numerator | ASC   | In order 20.0,42,40                                          |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value           | /numerator | DESC  | In order  40, 42 ,20.0                                       |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value/numerator |            | ASC   | In order 20.0,40,  42                                        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value/numerator |            | DESC  | In order 42 ,40 ,20.0                                        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/magnitude |            | ASC   | 42,50 ,400                                                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/magnitude |            | DESC  | 400  ,50,42                                                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value           | /magnitude | ASC   | 42,50 ,400                                                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value           | /magnitude | DESC  | 400  ,50,42                                                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value/value     |            | ASC   | 2022-02-03T04:05:06,2022-03-03T04:05:06 ,2023-02-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value/value     |            | DESC  | 2023-02-03T04:05:06,2022-03-03T04:05:06 ,2022-02-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value           | /value     | ASC   | 2022-02-03T04:05:06,2022-03-03T04:05:06 ,2023-02-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value           | /value     | DESC  | 2023-02-03T04:05:06,2022-03-03T04:05:06 ,2022-02-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value/value     |            | ASC   | 04:05:06,04:06:06,05:05:06                                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value/value     |            | DESC  | 05:05:06,04:06:06,04:05:06                                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value           | /value     | ASC   | 04:05:06,04:06:06,05:05:06                                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value           | /value     | DESC  | 05:05:06,04:06:06,04:05:06                                   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value/value     |            | ASC   | 2022-02-03,2022-03-03,2023-02-03                             |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value/value     |            | DESC  | 2023-02-03,2022-03-03,2022-02-03                             |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value           | /value     | ASC   | 2022-02-03,2022-03-03,2023-02-03                             |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value           | /value     | DESC  | 2023-02-03,2022-03-03,2022-02-03                             |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value/value     |            | ASC   | 1,1,2                                                        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value/value     |            | DESC  | 2,1,1                                                        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value           | /value     | ASC   | 1,1,2                                                        |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value           | /value     | DESC  | 2,1,1                                                        |


## Multiple Order BY

### Order BY on extracted columns IV

1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
3. Create ehr
4. Create composition  `aql-conformance-ehrbase.org.v0_contains.json`
5. Create composition  `type_repetition_conformance_ehrbase.org_one_reptation.json`
6. Run Query "SELECT c/name/value,o/name/value from EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o ORDER BY
   c/name/value {order1}, o/name/value {order2}"

| {order1} | {order2} | result in order                                                                                                                                                                          |
|----------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ASC      | ASC      | {aql-conformance-ehrbase.org.v0, Blood pressure },4 x { aql-conformance-ehrbase.org.v0 , Conformance Observation },{type_repetition_conformance_ehrbase.org,Conformance Observation }    |
| DESC     | DESC     | {type_repetition_conformance_ehrbase.org,Conformance Observation }, 4 x { aql-conformance-ehrbase.org.v0 , Conformance Observation } ,{aql-conformance-ehrbase.org.v0, Blood pressure }  |
| ASC      | DESC     | 4 x { aql-conformance-ehrbase.org.v0 , Conformance Observation } ,{aql-conformance-ehrbase.org.v0, Blood pressure }, {type_repetition_conformance_ehrbase.org,Conformance Observation }  |
| DESC     | ASC      | {type_repetition_conformance_ehrbase.org,Conformance Observation },  {aql-conformance-ehrbase.org.v0, Blood pressure }, 4 x { aql-conformance-ehrbase.org.v0 , Conformance Observation } |
