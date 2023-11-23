# FROM
## TEST roundtrip 
### Tests
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT c FROM COMPOSITION c`
5. Check c equal as JSON to  `conformance_ehrbase.de.v0.opt` minus uid
## TEST FROM Usable RM-types  [Usable RM-types A-D](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#Usable-RM-types)
### TEST FROM ehr
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT e/ehr_id/value FROM EHR e`
5. Check return one row with the ehrId 

### TEST FROM composition
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT t FROM {type} t`

| type                | expected result                                  |
|---------------------|--------------------------------------------------|
| COMPOSITION         | one result;JSON with _type = COMPOSITION         |
| EVENT_CONTEXT       | one result;JSON with _type = EVENT_CONTEXT       |
| SECTION             | one result;JSON with _type = SECTION             |
| ADMIN_ENTRY         | one result;JSON with _type = ADMIN_ENTRY         |
| OBSERVATION         | one result;JSON with _type = OBSERVATION         |
| INSTRUCTION         | one result;JSON with _type = INSTRUCTION         |
| ACTION              | one result;JSON with _type = ACTION              |
| EVALUATION          | one result;JSON with _type = EVALUATION          |
| INSTRUCTION_DETAILS | one result;JSON with _type = INSTRUCTION_DETAILS |
| ACTIVITY            | one result;JSON with _type = ACTIVITY            |


### TEST FROM Common types 
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json` 
4. Run Query 'Select `SELECT t FROM {type} t`
  
| type           | expected result                                                                            |
|----------------|--------------------------------------------------------------------------------------------|
| FEEDER_AUDIT   | 5 result;JSON with _type = FEEDER_AUDIT                                                    |
| HISTORY        | one result;JSON with name/value = History                                                  |
| POINT_EVENT    | one result;JSON with _type = POINT_EVENT                                                   |
| INTERVAL_EVENT | 2 result;JSON with _type = INTERVAL_EVENT                                                  |
| ITEM_TREE      | 400 status code; message: "It is unclear if ITEM_TREE targets a COMPOSITION or EHR_STATUS" |
| CLUSTER        | 400 status code; message: "It is unclear if CLUSTER targets a COMPOSITION or EHR_STATUS"   |


### TEST FROM abstract types
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT t FROM {type} t`

| type           | expected result                                                                                   |
|----------------|---------------------------------------------------------------------------------------------------|
| ENTRY          | 5 result;JSON with _type in (1 ADMIN_ENTRY, 1 OBSERVATION, 1 INSTRUCTION, 1 ACTION, 1 EVALUATION) |
| CARE_ENTRY     | 4 result;JSON with _type in ( 1 OBSERVATION, 1 INSTRUCTION, 1 ACTION, 1 EVALUATION)               |
| ITEM_STRUCTURE | 400 status code; message: "It is unclear if ITEM_STRUCTURE targets a COMPOSITION or EHR_STATUS"   |
| DATA_STRUCTURE | 400 status code; message starts with: "CONTAINS DATA_STRUCTURE is not supported"                  |

### TEST FROM ITEM_STRUCTURE and ELEMENT  in composition
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT t FROM COMPOSITION contains {type} t`

| type           | expected result                                        |
|----------------|--------------------------------------------------------|
| ITEM_TREE      | 14 result;JSON with _type = ITEM_TREE                  |
| CLUSTER        | 3 result;JSON with _type = CLUSTER                     |
| ITEM_STRUCTURE | 14 result;JSON with _type in ( 14 ITEM_TREE)           |
| DATA_STRUCTURE | 15 result;JSON with _type in (1 HISTORY ,14 ITEM_TREE) |
| ELEMENT        | 68 result;JSON with _type = ELEMENT                    |

## Test from EHR_STATUS
### test from single ehr
1. Create ehr with Status `status1.json` and save {ehr_id}
2. Run the Query `Select s/{path} from EHR e contains EHR_STATUS s where e/ehr_id/value = '{ehr_id}'`

| {path}                               | result                                                                                                                                              |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| is_modifiable                        | true                                                                                                                                                |
| subject                              | json: { "external_ref": { "id": { "_type": "GENERIC_ID", "value": "ins1920", "scheme": "id_scheme" }, "namespace": "examples", "type": "PERSON" } } |
| subject/external_ref/id/value        | ins1920                                                                                                                                             |
| other_details/items[at0001]/value/id | 55175056                                                                                                                                            |

### test from multiple ehrs
1. Create ehr with Status `status1.json` 
2. Create ehr with Status `status2.json` 
3. Run the Query `Select s/subject/external_ref/id/value, s/other_details/items[at0001]/value/id   from EHR e contains EHR_STATUS s`
4. check result `{ins1920,55175056}, {ins1921,55175057}`

### test via part 
1. Create ehr with Status `status1.json` and save {ehr_id}
2. Run the Query `Select e/ehr_status/{path} from EHR e where e/ehr_id/value = '{ehr_id}'`

| {path}                               | result                                                                                                                                              |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| is_modifiable                        | true                                                                                                                                                |
| subject                              | json: { "external_ref": { "id": { "_type": "GENERIC_ID", "value": "ins1920", "scheme": "id_scheme" }, "namespace": "examples", "type": "PERSON" } } |
| subject/external_ref/id/value        | ins1920                                                                                                                                             |
| other_details/items[at0001]/value/id | 55175056                                                                                                                                            |

### contains 

1. Upload `ehrbase_blood_pressure_simple.de.v0.opt` if not exist
2. Create ehr with Status `status1.json` and save {ehr_id}
3. Create composition `ehrbase_blood_pressure_simple.de.v0.json`
4. Run the Query `Select l/name/value from EHR e contains {contains}'`

| {contains}                    | result                                                                                                                                                 |
|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| EHR_STATUS contains ELEMENT l | "family group id"                                                                                                                                      |
| ELEMENT l [at0001]            | "family group id"                                                                                                                                      |
| ELEMENT l [at0013]            | "Cuff size"                                                                                                                                            |
| ELEMENT l                     | "family group id","Cuff size","Location of measurement","Korotkoff sounds", "Systolic", "Diastolic" ,"Mean Arterial Pressure","Mean Arterial Pressure" |


## Test FROM Predicate [Predicate A - D](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#Predicates)   

### Without Predicate 
1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Run Query 'Select `SELECT o FROM  OBSERVATION o`
5. Check return is 5 Observations 

###  Predicate on extracted column
1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Run Query 'Select `SELECT t FROM {type} t {predicate}`

| type        | predicate                                            | expected result                                                                                  |
|-------------|------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| OBSERVATION | [openEHR-EHR-OBSERVATION.conformance_observation.v0] | 4 result;JSON with archetype_node_id = openEHR-EHR-OBSERVATION.conformance_observation.v0        |
| OBSERVATION | [openEHR-EHR-OBSERVATION.blood_pressure.v2]          | 1 result;JSON with archetype_node_id = openEHR-EHR-OBSERVATION.blood_pressure.v2                 |
| SECTION     | [openEHR-EHR-SECTION.conformance_section.v0]         | 1 result;JSON with archetype_node_id = openEHR-EHR-SECTION.conformance_section.v0                |
| SECTION     | [openEHR-EHR-SECTION.adhoc.v1]                       | 2 result;JSON with archetype_node_id = openEHR-EHR-SECTION.adhoc.v1                              |
| SECTION     | [openEHR-EHR-SECTION.adhoc.v1,'Section 1']           | 1 result;JSON with archetype_node_id = openEHR-EHR-SECTION.adhoc.v1   and name/value='Section 1' |

## contains (FROM A-D)[https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#SELECT-([inlineCard])]
### contains plus contain chaining
1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Run Query 'Select `SELECT o FROM {from}`

| from                                                                                                  | expected result                                                                                                                                                                |
|-------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| COMPOSITION contains Observation o                                                                    | 5 results;JSON with _type = OBSERVATION                                                                                                                                        |
| SECTION contains Observation o                                                                        | 5 results;JSON with _type = OBSERVATION                                                                                                                                        |
| SECTION [openEHR-EHR-SECTION.adhoc.v1] contains Observation o                                         | 2 results;JSON with _type = OBSERVATION and uuid in {1 55415141-17e4-4c71-9429-aa0fe6694c83, 1 94c0e756-e892-4985-884b-46829605a236}                                           |
| SECTION [openEHR-EHR-SECTION.conformance_section.v0] contains Observation o                           | 3 results;JSON with _type = OBSERVATION and uuid in {1 2183807d-af68-41c5-9bfe-28cd150d62f7, 1 d4cccdfc-9c90-402f-b4bb-94e8dc4ea429, 1 893506a7-462b-40b8-9638-0aa3990642d9 }  |
| SECTION [openEHR-EHR-SECTION.conformance_section.v0] contains Observation o contains Cluster          | 3 results;JSON with _type = OBSERVATION and uuid in {1 893506a7-462b-40b8-9638-0aa3990642d9, 2 893506a7-462b-40b8-9638-0aa3990642d9, 3 2183807d-af68-41c5-9bfe-28cd150d62f7 }  |
| Observation o contains Cluster                                                                        | 3 results;JSON with _type = OBSERVATION and uuid in {1 893506a7-462b-40b8-9638-0aa3990642d9, 2 893506a7-462b-40b8-9638-0aa3990642d9, 3 2183807d-af68-41c5-9bfe-28cd150d62f7  } |
| Observation o contains INTERVAL_EVENT                                                                 | 2 results;JSON with _type = OBSERVATION  and uuid in {1 893506a7-462b-40b8-9638-0aa3990642d9 }                                                                                 |
| Observation o contains INTERVAL_EVENT contains CLUSTER                                                | 1 result;JSON with _type = OBSERVATION   and uuid in {1 d4cccdfc-9c90-402f-b4bb-94e8dc4ea429,1 893506a7-462b-40b8-9638-0aa3990642d9 }                                          |
| EVENT o contains CLUSTER                                                                              | 2 results;JSON with _type in {1 INTERVAL_EVENT,1 POINT_EVENT}                                                                                                                  |
| OBSERVATION [openEHR-EHR-OBSERVATION.blood_pressure.v2]  contains ELEMENT e[at0004]                   | 1 results;JSON with name/value = 'Systolic'                                                                                                                                    |
| COMPOSITION contains CLUSTER [openEHR-EHR-CLUSTER.conformance_cluster.v0]  contains ELEMENT e[at0003] | 3 results;JSON with name/value = 'labresult'                                                                                                                                   |

### contains with repeating types

1. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
2. Create ehr
3. Create composition `type_repetition_conformance_ehrbase.org_one_reptation.json`
4. Run Query 'Select `SELECT o FROM COMPOSITION contains {from}`

   | from                                 | expected result                                                                                                         |
   |--------------------------------------|-------------------------------------------------------------------------------------------------------------------------|
   | SECTION  o                           | 2 result;JSON with archetype_node_id in {1 openEHR-EHR-SECTION.adhoc.v1 , 1 openEHR-EHR-SECTION.conformance_section.v0} |
   | SECTION contains SECTION  o          | 1 result;JSON with archetype_node_id in { 1 openEHR-EHR-SECTION.conformance_section.v0}                                 |                                                                                                                                       
   | CLUSTER  o                           | 3 result;JSON with archetype_node_id in {1 openEHR-EHR-CLUSTER.cluster_with_cluster.v0 , 1 at0005, 1 at0003}            |
   | CLUSTER contains CLUSTER  o          | 2 result;JSON with archetype_node_id in { 1 at0005, 1 at0003}                                                           |
   | CLUSTER contains CLUSTER  o [at0005] | 1 result;JSON with archetype_node_id in { 1 at0005}                                                                     |

## 'AND' and 'OR'

### Simple 'AND' and 'OR'
1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Create ehr
3. Create composition `aql-conformance-ehrbase.org.v0_contains.json`
4. Run Query '{query}}`

| query                                                                                                                                                                                                                                                                                                                                                             | expected result                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SELECT o1/name/value,o2/name/value FROM EHR CONTAINS SECTION CONTAINS (OBSERVATION  o1[openEHR-EHR-OBSERVATION.conformance_observation.v0] AND OBSERVATION o2[openEHR-EHR-OBSERVATION.blood_pressure.v2])                                                                                                                                                         | {Conformance Observation,Blood pressure} ,{Conformance Observation,Blood pressure}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| SELECT o1/name/value,o2/name/value FROM EHR CONTAINS SECTION CONTAINS (OBSERVATION  o1[openEHR-EHR-OBSERVATION.conformance_observation.v0] OR OBSERVATION o2[openEHR-EHR-OBSERVATION.blood_pressure.v2])                                                                                                                                                          | {Conformance Observation,Blood pressure}, {Conformance Observation,Blood pressure},{Conformance Observation,null},{Conformance Observation,null}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| SELECT s1/name/value,o1/name/value,s2/name/value,o2/name/value FROM EHR CONTAINS ((SECTION s1[openEHR-EHR-SECTION.conformance_section.v0] CONTAINS OBSERVATION  o1[openEHR-EHR-OBSERVATION.conformance_observation.v0] ) OR (SECTION s2[openEHR-EHR-SECTION.adhoc.v1,'Section 1'] CONTAINS OBSERVATION  o2[openEHR-EHR-OBSERVATION.conformance_observation.v0] )) | 2x {"conformance section", "Conformance Observation", "Section 1", "Conformance Observation"}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| SELECT s1/name/value,o1/name/value,o2/name/value FROM EHR CONTAINS SECTION s1 CONTAINS (OBSERVATION  o1[openEHR-EHR-OBSERVATION.conformance_observation.v0] AND OBSERVATION o2[openEHR-EHR-OBSERVATION.blood_pressure.v2])                                                                                                                                        | 2 x {conformance section,Conformance Observation,Blood pressure}                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| SELECT s1/name/value,o1/name/value, c/name/value ,o2/name/value,o3/name/value FROM EHR CONTAINS (SECTION s1 CONTAINS ((OBSERVATION  o1 CONTAINS CLUSTER c )AND OBSERVATION o2[openEHR-EHR-OBSERVATION.blood_pressure.v2]) OR OBSERVATION[openEHR-EHR-OBSERVATION.conformance_observation.v0] o3)                                                                  | {"conformance section","Conformance Observation","conformance cluster","Blood pressure", "Conformance Observation"},  {"conformance section","Conformance Observation","conformance cluster","Blood pressure", "Conformance Observation"},  {"conformance section","Blood pressure","conformance cluster","Blood pressure", "Conformance Observation"},  {"conformance section","Conformance Observation","conformance cluster","Blood pressure", "Conformance Observation"},  {"conformance section","Conformance Observation","conformance cluster","Blood pressure", "Conformance Observation"},  {"conformance section","Blood pressure","conformance cluster","Blood pressure", "Conformance Observation"},  {"Section 1",null,null,null, "Conformance Observation"},  {"Section 2",null,null,null, "Conformance Observation"} |