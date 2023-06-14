# WHERE [Feature List](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#WHERE)
## Simple Compare
### Compare with extracted column I https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#WHERE
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Save ehr_id as {ehr_id_1}
4. Create composition  `conformance_ehrbase.de.v0_max.json`
5. Save comp_id as {comp_id_1}
6. Create ehr
7. Save ehr_id as {ehr_id_2}
8. Create composition  `conformance_ehrbase.de.v0_max.json`
9. Save comp_id as {comp_id_2}
10. Run Query "SELECT e/ehr_id/value, c/uid/value from EHR e CONTAINS COMPOSITION c WHERE {where}"

| {where}                      | e/ehr_id/value | c/uid/value |
|------------------------------|----------------|-------------|
| e/ehr_id/value = {ehr_id_1}  | {ehr_id_1}     | {comp_id_1} |
| e/ehr_id/value = {ehr_id_2}  | {ehr_id_2}     | {comp_id_2} |
| e/ehr_id/value != {ehr_id_1} | {ehr_id_2}     | {comp_id_2} |
| e/ehr_id/value != {ehr_id_2} | {ehr_id_1}     | {comp_id_1} |
| c/uid/value = {comp_id_1}    | {ehr_id_1}     | {comp_id_1} |
| c/uid/value = {comp_id_2}    | {ehr_id_2}     | {comp_id_2} |
| c/uid/value != {comp_id_1}   | {ehr_id_2}     | {comp_id_2} |
| c/uid/value != {comp_id_2}   | {ehr_id_1}     | {comp_id_1} |


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



