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
11. Run Query "SELECT c/name/value from EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o ORDER BY  {order}"

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
8. Run Query "SELECT o/name/value from EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o ORDER BY  {order}"

| {order}                 | result in order                                    |
|-------------------------|----------------------------------------------------|
| o/name/value ASC        | 2 x  Blood pressure, 5 x  Conformance Observation  |
| o/name/value DESC       | 5 x  Conformance Observation, 2 x  Blood pressure, |

### Order BY on extracted columns III
1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
3. Create ehr with {ehr_id1} = 5840970c-feb7-47d8-afca-12bfc8486b81
4. Create composition  `aql-conformance-ehrbase.org.v0_contains.json` with comp_id set to {comp_id_1} = 9b9725a1-347a-40dd-8c68-e78938ec0f19::local.ehrbase.org::1
5. Create ehr with {ehr_id2} = cc779563-6505-4f87-9e6b-c9ba5dc887fb
6. Create composition  `type_repetition_conformance_ehrbase.org_one_reptation.json` with comp_id set to {comp_id_2} = a9ac5dd6-6cd6-4adb-9eb5-586930ae13e9::local.ehrbase.org::1
7. Run Query "SELECT {path} from EHR e CONTAINS COMPOSITION c  ORDER BY {path} {order}"

| {path}                                | {order} | result in order                                                                                                        |
|---------------------------------------|---------|------------------------------------------------------------------------------------------------------------------------|
| c/ehr_id/value                        | ASC     | 5840970c-feb7-47d8-afca-12bfc8486b81, cc779563-6505-4f87-9e6b-c9ba5dc887fb                                             |
| c/ehr_id/value                        | DESC    | cc779563-6505-4f87-9e6b-c9ba5dc887fb, 5840970c-feb7-47d8-afca-12bfc8486b81                                             |
| c/archetype_details/template_id/value | ASC     | aql-conformance-ehrbase.org.v0, type_repetition_conformance_ehrbase.org                                                |
| c/archetype_details/template_id/value | DESC    | type_repetition_conformance_ehrbase.org, aql-conformance-ehrbase.org.v0                                                |
| c/uid/value                           | ASC     | 9b9725a1-347a-40dd-8c68-e78938ec0f19::local.ehrbase.org::1, a9ac5dd6-6cd6-4adb-9eb5-586930ae13e9::local.ehrbase.org::1 |
| c/uid/value                           | DESC    | a9ac5dd6-6cd6-4adb-9eb5-586930ae13e9::local.ehrbase.org::1, 9b9725a1-347a-40dd-8c68-e78938ec0f19::local.ehrbase.org::1 |

### Order BY on extracted columns IV
1. Upload `aql-conformance-ehrbase.org.v0.opt` if not exist
2. Upload `type_repetition_conformance_ehrbase.org.opt` if not exist
3. Create ehr with {ehr_id1} = 5840970c-feb7-47d8-afca-12bfc8486b81
4. Create composition  `aql-conformance-ehrbase.org.v0_contains.json` with comp_id set to {comp_id_1} = 9b9725a1-347a-40dd-8c68-e78938ec0f19::local.ehrbase.org::1
5. Create ehr with {ehr_id2} = cc779563-6505-4f87-9e6b-c9ba5dc887fb
6. Create composition  `type_repetition_conformance_ehrbase.org_one_reptation.json` with comp_id set to {comp_id_2} = a9ac5dd6-6cd6-4adb-9eb5-586930ae13e9::local.ehrbase.org::1
7. Run Query "SELECT {path} from EHR e CONTAINS COMPOSITION c CONTAINS OBSERVATION o  ORDER BY {path} {order}"

| {path}              | {order} | result in order                                                                                        |
|---------------------|---------|--------------------------------------------------------------------------------------------------------|
| o/archetype_node_id | ASC     | 2 x openEHR-EHR-OBSERVATION.blood_pressure.v2 , 5 x openEHR-EHR-OBSERVATION.conformance_observation.v0 |
| o/archetype_node_id | DESC    | 5 x openEHR-EHR-OBSERVATION.conformance_observation.v0, 2 x openEHR-EHR-OBSERVATION.blood_pressure.v2  |
