# VERSION

## VERSION\[LATEST_VERSION\] CONTAINS EHR_STATUS

1. Create ehr #1 and Save ehr_id as `{ehr_id_1}`
   1. Get ehr_status for ehr #1 with `{ehr_id_1}` and save id as `{ehr_status_id_1}`
2. Create ehr #2 and Save ehr_id as `{ehr_id_2}`
   1. Get ehr_status for ehr #2 with `{ehr_id_2}` and save id as `{ehr_status_id_2}`
3. Delete ehr #2 with `{ehr_id_2}`
4. Create ehr #3 and Save ehr_id as `{ehr_id_3}`
   1. Get ehr_status for ehr #3 with `{ehr_id_3}` and save id as {ehr_status_id_3}
5. Update ehr status #3 with {ehr_status_id_3} set `subject`
   ```json 
   "subject": {
     "external_ref": {
       "id": {
         "_type": "GENERIC_ID",
         "value": "ext_id_42",
         "scheme": "id_scheme"
       },
       "namespace": "examples",
       "type": "PERSON"
     }
   }
   ```

### Sanity check
1. Run Query
   ```sql
   SELECT
     cv/uid/value,
     cv/commit_audit/time_committed/value,
     cv/commit_audit/change_type/value,
     cv/commit_audit/change_type/defining_code/code_string,
     cv/commit_audit/change_type/defining_code/preferred_term,
     cv/commit_audit/change_type/defining_code/terminology_id/value,
     s/subject/external_ref/id/value
   FROM VERSION cv[LATEST_VERSION] 
     CONTAINS EHR_STATUS s
   ORDER BY
     cv/commit_audit/time_committed ASC
   ```
2. Check result
   ```json
   [
     [
       "ae6432e3-aa7a-41c6-93b6-0589071502c9::local.ehrbase.org::1",
       "2024-03-04T09:07:23.894303+01:00",
       "creation",
       "249",
       "creation",
       "openehr",
       null
     ],
     [
       "cf6a6118-7e25-4837-b99c-0fa134fa4396::local.ehrbase.org::2",
       "2024-03-04T09:11:01.225664+01:00",
       "modification",
       "251",
       "modification",
       "openehr",
       "ext_id_42"
     ]
   ]
   ```

### Contribution check

Contributions for all 2 ehr_status objects.

1. Run Query
   ```sql
   SELECT
	cv/uid/value,
    cv/contribution/id/value,
    cv/commit_audit/time_committed/value
   FROM VERSION cv[LATEST_VERSION] 
     CONTAINS EHR_STATUS s
   ORDER BY
     cv/commit_audit/time_committed ASC
   ```
2. Obtain contribution ids 
   1. row[0] `{ehr_id_1}` cv/uid/value as `{ehr_status_id_1}` and cv/contribution/id/value as {contrib_id_1}
   2. row[1] `{ehr_id_2}` cv/uid/value as `{ehr_status_id_2}` and cv/contribution/id/value as {contrib_id_2}
3. Verify contributions exist and valid
   1. `GET rest/openehr/v1/ehr/{ehr_id_1}/contribution/{contrib_id_1}` versions/value == `{ehr_status_id_1}`
   2. `GET rest/openehr/v1/ehr/{ehr_id_2}/contribution/{contrib_id_2}` versions/value == ``{ehr_status_id_2}``

### Data values

```sql
   SELECT
     {path}
   FROM VERSION cv[LATEST_VERSION] 
     CONTAINS EHR_STATUS s
   {where} 
   {order_by}
```

| {path}                                                         | {where}                              | {order_by}            | result                                                                                             |
|----------------------------------------------------------------|--------------------------------------|-----------------------|----------------------------------------------------------------------------------------------------|
| cv/uid/value                                                   | WHERE {path} = '`{ehr_status_id_3}`' | ORDER BY {path}  DESC | `{ehr_status_id_3}`                                                                                |
| cv/commit_audit/system_id                                      | WHERE {path} = 'local.ehrbase.org'   |                       | ["local.ehrbase.org"], ["local.ehrbase.org"]                                                       |
| cv/commit_audit/change_type                                    |                                      |                       | [./version/status.version.latest.change_type.json](version/status.version.latest.change_type.json) |
| cv/commit_audit/change_type/value                              | WHERE {path} = 'modification'        | ORDER BY {path}       | modification                                                                                       |
| cv/commit_audit/change_type/defining_code/code_string          | WHERE {path} = '249'                 | ORDER BY {path} ASC   | 249                                                                                                |  
| cv/commit_audit/change_type/defining_code/preferred_term       | WHERE {path} = 'creation'            | ORDER BY {path}  DESC | creation                                                                                           |
| cv/commit_audit/change_type/defining_code/terminology_id/value |                                      |                       | ["openehr"], ["openehr"]                                                                           |


## VERSION\[LATEST_VERSION\] CONTAINS COMPOSITION

### Fixture Setup
1. Upload `persistent_minimal.opt` if not exist
2. Upload `conformance_ehrbase.de.v0.opt` if not exist
3. Create ehr #1 as `{ehr_id_1}`
   1. Create composition #1.1 via contribution with commit audit using `minimal_persistent.contribution.json` and `versions[0].id.value` as `{comp_id_1_1}`
   2. Create composition #1.2 `persistent_minimal.en.v1__full.json` as `{comp_id_1_2}`
   3. Update composition #1.2 with `{comp_id_1_2}` `at0001/at0002/at0003/at0004/value = "Updated Text for version 2"` to version 2
   4. Update composition #1.2 with `{comp_id_1_2}` `at0001/at0002/at0003/at0004/value = "Updated Text for version 3"` to version 3
   5. Create composition #1.3 `persistent_minimal.en.v1__full.json` as `{comp_id_1_3}`
   6. Update composition #1.3 with `{comp_id_1_3}` `at0001/at0002/at0003/at0004/value = "Updated Text for version 2"` to version 2
   7. Create composition #1.4 `persistent_minimal.en.v1__full.json` as `{comp_id_1_4}`
   8. Delete composition #1.4 with `{comp_id_1_4}`
   9. Create composition #1.5 `conformance_ehrbase.de.v0_max.json`  as `{comp_id_1_5}`
4. Create ehr #2 as `{ehr_id_2}`
   1. Create composition #2.1 `persistent_minimal.en.v1__full.json` as `{comp_id_2_1}`
   2. Delete composition #2.2 with `{comp_id_2_1}`
   3. Delete ehr #2
5. Create ehr #3 as `{ehr_id_3}`
   1. Get ehr_status for ehr #3 with `{ehr_id_3}` and save id as `{ehr_status_id_3}`
6. Update ehr #3 with `{ehr_status_id_3}` to version 2 set `subject`
   ```json 
   "subject": {
     "external_ref": {
       "id": {
         "_type": "GENERIC_ID",
         "value": "ext_id_42",
         "scheme": "id_scheme"
       },
       "namespace": "examples",
       "type": "PERSON"
     }
   }
   ```
   1. Create composition #3.1 `persistent_minimal.en.v1__full.json` as `{comp_id_3_1}`
   2. Update composition #3.1 with `{comp_id_3_1}` `at0001/at0002/at0003/at0004/value = "ehr 3 composition version 2"` to version 2
   3. Create composition #3.2 `conformance_ehrbase.de.v0_max.json` as `{comp_id_3_2}`

Fixture dataset is now (ordered by time_committed ASC)

```yaml
[ehr #1 / composition #1.1]::local.ehrbase.org::1 (openEHR-EHR-COMPOSITION.persistent_minimal.v1)
[ehr #1 / composition #1.2]::local.ehrbase.org::3 (openEHR-EHR-COMPOSITION.persistent_minimal.v1)
[ehr #1 / composition #1.3]::local.ehrbase.org::2 (openEHR-EHR-COMPOSITION.persistent_minimal.v1)
[ehr #1 / composition #1.4]::local.ehrbase.org::1 (openEHR-EHR-COMPOSITION.conformance_composition_.v0)

[ehr #2 / DELETED] 

[ehr #3 / composition #3.1]::local.ehrbase.org::2 (openEHR-EHR-COMPOSITION.persistent_minimal.v1)
[ehr #3 / composition #3.2]::local.ehrbase.org::1 (openEHR-EHR-COMPOSITION.conformance_composition_.v0)
```

### Sanity check
1. Run Query
   ```sql
   SELECT
     e/ehr_id/value,
     c/archetype_details/template_id/value,
     cv/uid/value,
     cv/commit_audit/time_committed/value
   FROM EHR e
     CONTAINS VERSION cv[LATEST_VERSION]
     CONTAINS COMPOSITION c
   ORDER BY
     cv/commit_audit/time_committed
   ```
2. Check result
   ```json
   [
     [
       {ehr_id_1},
       "persistent_minimal.en.v1",
       {comp_id_1_1},
       "2024-03-04T13:24:38.335587+01:00"
     ],
     [
       {ehr_id_1},
       "persistent_minimal.en.v1",
       {comp_id_1_2}",
       "2024-03-04T13:29:27.973907+01:00"
     ],
     [
       {ehr_id_1},
       "persistent_minimal.en.v1",
       {comp_id_1_3},
       "2024-03-04T13:30:54.757872+01:00"
     ],
     [
       {ehr_id_1},
       "conformance-ehrbase.de.v0",
       {comp_id_1_5},
       "2024-03-04T13:33:11.637201+01:00"
     ],
     [
       {ehr_id_3},
       "persistent_minimal.en.v1",
       {comp_id_3_1},
       "2024-03-04T13:43:27.198659+01:00"
     ],
     [
       {ehr_id_3},
       "conformance-ehrbase.de.v0",
       {como_id_3_2},
       "2024-03-04T13:48:27.589896+01:00"
     ]
   ]
   ```

### Contribution check

Contributions for the 2 compositions created for template `openEHR-EHR-COMPOSITION.conformance_composition_.v0`.

1. Run Query
   ```sql
   SELECT
     cv/uid/value,
     cv/contribution/id/value,
     cv/commit_audit/time_committed/value
   FROM VERSION cv[LATEST_VERSION]
     CONTAINS COMPOSITION c[openEHR-EHR-COMPOSITION.conformance_composition_.v0]
   ORDER BY
     cv/commit_audit/time_committed ASC
   ```
2. Obtain contribution ids
   1. row[0] `{ehr_id_1}` cv/uid/value as `{comp_id_1_5}` and cv/contribution/id/value as {contrib_id_1}
   2. row[1] `{ehr_id_3}` cv/uid/value as {como_id_3_2} and cv/contribution/id/value as {contrib_id_2}
3. Verify contributions exist and valid
   1. `GET rest/openehr/v1/ehr/{ehr_id_1}/contribution/{comp_id_1_5}` versions/value == `{comp_id_1_5}`
   2. `GET rest/openehr/v1/ehr/{ehr_id_3}/contribution/{como_id_3_2}` versions/value == {como_id_3_2}

### Data values

Run tests for path value on compositions for template `openEHR-EHR-COMPOSITION.persistent_minimal.v1`. 

```sql
   SELECT
     {path}
   FROM VERSION cv[LATEST_VERSION] 
     CONTAINS COMPOSITION c[openEHR-EHR-COMPOSITION.persistent_minimal.v1]
   {where} 
   {order_by}
```

| {path}                                                         | {where}                                             | {order_by}           | result                                                                                                         |
|----------------------------------------------------------------|-----------------------------------------------------|----------------------|----------------------------------------------------------------------------------------------------------------|
| cv/uid/value                                                   | WHERE {path} = '`{comp_id_1_2}`'                    | ORDER BY {path}      | `{comp_id_1_2}`                                                                                                |
| cv/commit_audit/system_id                                      | WHERE cv/commit_audit/system_id  > 'local.'         |                      | ["local.ehrbase.org"], ["local.ehrbase.org"], ["local.ehrbase.org"], ["local.ehrbase.org"]                     |
| cv/commit_audit/change_type                                    |                                                     |                      | [./version/composition.version.latest.change_type.json](version/composition.version.latest.change_type.json)   |
| cv/commit_audit/change_type/value                              | WHERE {path} = 'creation'                           | ORDER BY {path}      | creation                                                                                                       |
| cv/commit_audit/change_type/defining_code/code_string          | WHERE {path} = '251'                                | ORDER BY {path} DESC | ["251"], ["251"], ["251"]                                                                                      |  
| cv/commit_audit/change_type/defining_code/preferred_term       | WHERE {path} = 'creation'                           | ORDER BY {path}      | creation                                                                                                       |
| cv/commit_audit/change_type/defining_code/terminology_id/value | WHERE {path}  > 'open'                              |                      | ["openehr"], ["openehr"], ["openehr"], ["openehr"]                                                             |
| cv/uid/value, cv/commit_audit/description                      | WHERE cv/uid/value = '`{comp_id_1_1}`'              |                      | [./version/composition.version.latest.commit_audit.json](version/composition.version.latest.commit_audit.json) |
| cv/commit_audit/description                                    | WHERE {path} = '&lt;optional audit description&gt;' | ORDER BY {path} ASC  | <optional audit description>                                                                                   |


## VERSION\[LATEST_VERSION\] CONTAINS OBSERVATION

### Fixture Setup

[Use the same fixture set as in VERSION\[LATEST_VERSION\] CONTAINS COMPOSITION](#versionlatest_version-contains-composition)

### Sanity check
1. Run Query
   ```sql
   SELECT
     e/ehr_id/value,
     o/archetype_details/template_id/value,
     o/archetype_details/archetype_id/value,
     cv/uid/value,
     cv/commit_audit/time_committed/value,
     o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value
   FROM EHR e
     CONTAINS VERSION cv[LATEST_VERSION]
     CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.minimal.v1]
   ORDER BY
     cv/commit_audit/time_committed
   LIMIT 2
   OFFSET 1
   ```
2. Check result
   ```json
   [
     [
       {ehr_id_1},
       "persistent_minimal.en.v1",
       "openEHR-EHR-OBSERVATION.minimal.v1",
       {comp_id_1_2},
       "2024-03-04T13:29:27.973907+01:00",
       "Updated Text for version 3"
     ],
     [
       {ehr_id_1},
       "persistent_minimal.en.v1",
       "openEHR-EHR-OBSERVATION.minimal.v1",
       {comp_id_1_3},
       "2024-03-04T13:30:54.757872+01:00",
       "Updated Text for version 2"
     ]
   ]
   ```

### Contribution check#

Contributions for the 42 compositions created for observation `openEHR-EHR-OBSERVATION.minimal.v1`.

1. Run Query
   ```sql
   SELECT
     e/ehr_id/value,
     cv/uid/value,
     cv/contribution/id/value,
     cv/commit_audit/time_committed/value
   FROM EHR e
     CONTAINS VERSION cv[LATEST_VERSION]
     CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.minimal.v1]
   ORDER BY
     cv/commit_audit/time_committed ASC
   ```
2. Obtain contribution ids
   1. row[0] `{ehr_id_1}` cv/uid/value as `{comp_id_1_1}` and cv/contribution/id/value as {contrib_id_1}
   2. row[1] `{ehr_id_1}` cv/uid/value as `{comp_id_1_2}` and cv/contribution/id/value as {contrib_id_2}
   3. row[2] `{ehr_id_1}` cv/uid/value as `{comp_id_1_3}` and cv/contribution/id/value as {contrib_id_3}
   4. row[2] {ehr_id_3} cv/uid/value as `{comp_id_3_1}` and cv/contribution/id/value as {contrib_id_4}
3. Verify contributions exist and valid
   1. `GET rest/openehr/v1/ehr/{ehr_id_1}/contribution/{contrib_id_1}` versions/value == `{comp_id_1_1}`
   2. `GET rest/openehr/v1/ehr/{ehr_id_1}/contribution/{contrib_id_2}` versions/value == `{comp_id_1_2}`
   3. `GET rest/openehr/v1/ehr/{ehr_id_1}/contribution/{contrib_id_3}` versions/value == `{comp_id_1_3}`
   4. `GET rest/openehr/v1/ehr/{ehr_id_3}/contribution/{contrib_id_4}` versions/value == `{comp_id_3_1}`

### Data values

Run tests for path value on compositions for template `openEHR-EHR-COMPOSITION.persistent_minimal.v1`.

```sql
   SELECT
     {path}
   FROM VERSION cv[LATEST_VERSION] 
     CONTAINS OBSERVATION o[openEHR-EHR-OBSERVATION.minimal.v1]
   {where} 
   {order_by}
```

| {path}                                                         | {where}                                              | {order_by}           | result                                                                                                         |
|----------------------------------------------------------------|------------------------------------------------------|----------------------|----------------------------------------------------------------------------------------------------------------|
| cv/uid/value                                                   | WHERE {path} = '`{comp_id_3_1}`'                     | ORDER BY {path}      | {comp_id_3_1}                                                                                                  |
| cv/commit_audit/system_id                                      | WHERE cv/commit_audit/system_id  > 'local.'          |                      | ["local.ehrbase.org"], ["local.ehrbase.org"], ["local.ehrbase.org"], ["local.ehrbase.org"]                     |
| cv/commit_audit/change_type                                    | WHERE cv/commit_audit/change_type/value = 'creation' |                      | [./version/observation.version.latest.change_type.json](version/observation.version.latest.change_type.json)   |
| cv/commit_audit/change_type/value                              | WHERE {path} = 'creation'                            | ORDER BY {path} DESC | creation                                                                                                       |
| cv/commit_audit/change_type/defining_code/code_string          | WHERE {path} = '249'                                 | ORDER BY {path} ASC  | 249                                                                                                            |  
| cv/commit_audit/change_type/defining_code/preferred_term       | WHERE {path} = 'creation'                            | ORDER BY {path}      | creation                                                                                                       |
| cv/commit_audit/change_type/defining_code/terminology_id/value | WHERE {path}  > 'open'                               |                      | ["openehr"], ["openehr"], ["openehr"], ["openehr"]                                                             |
| cv/uid/value, cv/commit_audit/description                      | WHERE cv/uid/value = '`{comp_id_1_1}`'               |                      | [./version/composition.version.latest.commit_audit.json](version/composition.version.latest.commit_audit.json) |
| cv/commit_audit/description                                    | WHERE {path} = '&lt;optional audit description&gt;'  | ORDER BY {path} DESC | <optional audit description>                                                                                   |
