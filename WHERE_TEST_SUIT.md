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



