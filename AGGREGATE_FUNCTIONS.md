# AGGREGATE FUNCTIONS

## Count
### count on ehr
1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
5. Create ehr
6. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
7. Run Query 'Select COUNT(e/ehr_id/value) from EHR e contains COMPOSITION' 
8. Check Result = 3
9. Run Query 'Select COUNT(DISTINCT e/ehr_id/value) from EHR e contains COMPOSITION'
10. Check Result = 2

### count on paths
1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v3.json`
4. Run Query 'Select `SELECT COUNT({path}) FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0]`

| path                                                                         | result |
|------------------------------------------------------------------------------|--------|
| *                                                                            | 1      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value               | 1      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value         | 3      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0005]/value               | 3      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0005]/value/value         | 3      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value               | 2      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/units         | 2      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude     | 2      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/null_flavour/value  | 1      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]                     | 3      |

## Non count aggregate functions on paths with known type


### MIN
1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v3.json`
4. Run Query 'Select `SELECT MIN({path}) FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0]`

| path                                                                        | result              |
|-----------------------------------------------------------------------------|---------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value        | "Lorem ipsum"       |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0005]/value/value        | "term1"             |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/units        | "mm"                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude    | 22.0                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/null_flavour/value | "unknown"           |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/magnitude    | 42                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value/value        | 2022-02-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value/value        | 04:05:06            |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value/value        | 2022-02-03          |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0018]/value/value        | "PT0S"              |

### MAX
1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v3.json`
4. Run Query 'Select `SELECT MAX({path}) FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0]`

| path                                                                        | result              |
|-----------------------------------------------------------------------------|---------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value        | "Lorem ipsum3"      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0005]/value/value        | "term1"             |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/units        | "mm"                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude    | 82.0                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/null_flavour/value | "unknown"           |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/magnitude    | 400,                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value/value        | 2023-02-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value/value        | 05:05:06            |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value/value        | 2023-02-03          |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0018]/value/value        | "PT6M40S"           |

### SUM
1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v3.json`
4. Run Query 'Select `SELECT SUM({path}) FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0]`

| path                                                                     | result |
|--------------------------------------------------------------------------|--------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude | 104.0  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/magnitude | 493    |

### AVG
1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v3.json`
4. Run Query 'Select `SELECT AVG({path}) FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0]`

| path                                                                     | result               |
|--------------------------------------------------------------------------|----------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude | 52.0                 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/magnitude | 164.3333333333333333 |


## aggregate function and where
### MAX
1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT MAX({path}) FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0] where {path} != {where}`

| path                                                                        | where          | result              |
|-----------------------------------------------------------------------------|----------------|---------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value        | "Lorem ipsum3" | "Lorem ipsum2"      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0005]/value/value        | "term1"        | null                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value/magnitude    | 400            | 42                  |

## aggregate function on DV_ORDERED
### MIN ON DV_ORDERED
1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v3.json`
4. Run Query 'Select `SELECT MAX({path}) FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0]`

| path                                                           | value                             | result                                         |
|----------------------------------------------------------------|-----------------------------------|------------------------------------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value | DV_QUANTITY(22.0 mm)              | expected_min_on_dv_ordered_at8_quantity.json   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value | DV_PROPORTION(20.0/2.0)           | expected_min_on_dv_ordered_at9_proportion.json |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value | DV_COUNT(2)                       | expected_min_on_dv_ordered_at10_count.json     |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value | DV_DATE_TIME(2022-02-03T04:05:06) | expected_min_on_dv_ordered_at11_datetime.json  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value | DV_TIME(04:05:06)                 | expected_min_on_dv_ordered_at12_time.json      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value | DV_DATE(2022-02-03)               | expected_min_on_dv_ordered_at13_date.json      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value | DV_ORDINAL(1 external:at0015)     | expected_min_on_dv_ordered_at14_ordinal.json   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0018]/value | DV_DURATION(PT0S)                 | expected_min_on_dv_ordered_at18_duration.json  |

### MAX ON DV_ORDERED
1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max_v3.json`
4. Run Query 'Select `SELECT MAX({path}) FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0]`

| path                                                           | value                             | result                                         |
|----------------------------------------------------------------|-----------------------------------|------------------------------------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value | DV_QUANTITY(82.0 mm)              | expected_max_on_dv_ordered_at8_quantity.json   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value | DV_PROPORTION(40.0/2.0)           | expected_max_on_dv_ordered_at9_proportion.json |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value | DV_COUNT(400)                     | expected_max_on_dv_ordered_at10_count.json     |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value | DV_DATE_TIME(2023-02-03T04:05:06) | expected_max_on_dv_ordered_at11_datetime.json  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value | DV_TIME(05:05:06)                 | expected_max_on_dv_ordered_at12_time.json      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value | DV_DATE(2023-02-03)               | expected_max_on_dv_ordered_at13_date.json      |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value | DV_ORDINAL(2 local:at0015)        | expected_max_on_dv_ordered_at14_ordinal.json   |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0018]/value | DV_DURATION(PT0S)                 | expected_max_on_dv_ordered_at18_duration.json  |

### AVG ON DV_ORDERED
1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Run Query 'Select `SELECT AVG({path}) FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0]`

| path                                                           | type          | result                                         |
|----------------------------------------------------------------|---------------|------------------------------------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value | DV_QUANTITY   | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value | DV_PROPORTION | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value | DV_COUNT      | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value | DV_DATE_TIME  | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value | DV_TIME       | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value | DV_DATE       | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value | DV_ORDINAL    | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0018]/value | DV_DURATION   | SKIP : Skipped due to 400 and Not implemented. |

### SUM ON DV_ORDERED
1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Run Query 'Select `SELECT SUM({path}) FROM OBSERVATION o [openEHR-EHR-OBSERVATION.conformance_observation.v0]`

| path                                                           | type          | result                                         |
|----------------------------------------------------------------|---------------|------------------------------------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value | DV_QUANTITY   | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0009]/value | DV_PROPORTION | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0010]/value | DV_COUNT      | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value | DV_DATE_TIME  | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0012]/value | DV_TIME       | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0013]/value | DV_DATE       | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0014]/value | DV_ORDINAL    | SKIP : Skipped due to 400 and Not implemented. |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0018]/value | DV_DURATION   | SKIP : Skipped due to 400 and Not implemented. |
