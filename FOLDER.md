# FOLDER

## FROM Folder


### Find all

1. Create ehr
2. Create directory `folder_simple_hierarchy`
3. Run Query `SELECT f/uid/value, f/name/value, f/archetype_node_id FOLDER f`
4. check result is in any order

| f/uid/value                          | f/name/value  | f/archetype_node_id                   |
|--------------------------------------|---------------|---------------------------------------|
| 10e952ca-a5b2-4f24-8d37-59240fd37020 | root1         | openEHR-EHR-FOLDER.generic.v1         |
| d936409e-901f-4994-8d33-ed104d460151 | subfolder1    | openEHR-EHR-FOLDER.generic.v1         |
| 0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3 | subsubfolder1 | openEHR-EHR-FOLDER.episode_of_care.v1 |


### Find by ehr id

1. Create ehr and save {ehr_id1}
2. Create directory `folder_simple_hierarchy` in {ehr_id1}
3. Create ehr and save {ehr_id2}
4. Create directory `folder_simple` in {ehr_id2}
5. Run Query `SELECT e/ehr_id/value, f/uid/value FROM EHR e CONTAINS FOLDER f WHERE e/ehr_id/value = {ehr_id1}`
6. check result is in any order

| e/ehr_id/value | f/uid/value                          |
|----------------|--------------------------------------|
| {ehr_id1}      | 10e952ca-a5b2-4f24-8d37-59240fd37020 |
| {ehr_id1}      | d936409e-901f-4994-8d33-ed104d460151 |
| {ehr_id1}      | 0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3 |


### Find by name

1. Create ehr
2. Create directory `folder_simple_hierarchy`
3. Run Query `SELECT f/uid/value from FOLDER f where f/name/value = {name}`
4. check for each parameter

| {name}        | f/uid/value                          |
|---------------|--------------------------------------|
| root1         | 10e952ca-a5b2-4f24-8d37-59240fd37020 |
| subfolder1    | d936409e-901f-4994-8d33-ed104d460151 |
| subsubfolder1 | 0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3 |


### Find by archetype

1. Create ehr
2. Create directory `folder_simple_hierarchy`
3. Run Query `SELECT f/uid/value from FOLDER f[openEHR-EHR-FOLDER.episode_of_care.v1]`
4. Check result one row with 0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3


### Find by Folder id

1. Create ehr
2. Create directory `folder_simple_hierarchy`
3. Run Query `SELECT f/name/value, f/uid/value FROM FOLDER f WHERE f/uid/value = '0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3'`
4. Check result matches single row

| f/name/value  | f/uid/value                          |
|---------------|--------------------------------------|
| subsubfolder1 | 0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3 |



## FOLDER contains FOLDER

### Find all

1. Create ehr
2. Create directory `folder_complex_hierarchy`
3. Run Query "SELECT f1/uid/value, f1/name/value,f2/uid/value, f2/name/value from FOLDER f1 contains Folder f2"
4. Check result contains in any order

| f1/uid/value                         | f1/name/value | f2/uid/value                         | f2/name/value |
|--------------------------------------|---------------|--------------------------------------|---------------|
| 10e952ca-a5b2-4f24-8d37-59240fd37020 | root1         | d936409e-901f-4994-8d33-ed104d460151 | subfolder1    |
| 10e952ca-a5b2-4f24-8d37-59240fd37020 | root1         | 0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3 | subsubfolder1 |
| 10e952ca-a5b2-4f24-8d37-59240fd37020 | root1         | 3cb9efa5-fe71-49e9-a02f-d38835c27d1b | subsubfolder2 |
| 10e952ca-a5b2-4f24-8d37-59240fd37020 | root1         | ac1e12cb-b9db-4f15-a8aa-4e62a9d97231 | subfolder2    |
| 10e952ca-a5b2-4f24-8d37-59240fd37020 | root1         | 13661fe2-1e16-4c75-a10d-9b8040487a72 | subsubfolder1 |
| 10e952ca-a5b2-4f24-8d37-59240fd37020 | root1         | e9ae5700-d969-430a-b4f2-445e3091a901 | subsubfolder2 |
| d936409e-901f-4994-8d33-ed104d460151 | subfolder1    | 0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3 | subsubfolder1 |
| d936409e-901f-4994-8d33-ed104d460151 | subfolder1    | 3cb9efa5-fe71-49e9-a02f-d38835c27d1b | subsubfolder2 |
| ac1e12cb-b9db-4f15-a8aa-4e62a9d97231 | subfolder2    | 13661fe2-1e16-4c75-a10d-9b8040487a72 | subsubfolder1 |
| ac1e12cb-b9db-4f15-a8aa-4e62a9d97231 | subfolder2    | e9ae5700-d969-430a-b4f2-445e3091a901 | subsubfolder2 |


### Find by

1. Create ehr
2. Create directory `folder_complex_hierarchy`
3. Run Query `SELECT f2/uid/value, f2/name/value FROM FOLDER f1[{predicate1}] CONTAINS FOLDER f2[{predicate2}]`

| {predicate1}                               | {predicate2}                                          | Result                                                                                                                                                                                                                     |
|--------------------------------------------|-------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| openEHR-EHR-FOLDER.generic.v1,'root1'      | openEHR-EHR-FOLDER.episode_of_care.v1                 | {0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3 , subsubfolder1}; {3cb9efa5-fe71-49e9-a02f-d38835c27d1b, subsubfolder2};{13661fe2-1e16-4c75-a10d-9b8040487a72, subsubfolder1 };{e9ae5700-d969-430a-b4f2-445e3091a901,subsubfolder2 } |
| openEHR-EHR-FOLDER.generic.v1,'subfolder1' | openEHR-EHR-FOLDER.episode_of_care.v1                 | {0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3 , subsubfolder1}; {3cb9efa5-fe71-49e9-a02f-d38835c27d1b, subsubfolder2}                                                                                                              |
| openEHR-EHR-FOLDER.generic.v1,'subfolder2' | openEHR-EHR-FOLDER.episode_of_care.v1                 | {13661fe2-1e16-4c75-a10d-9b8040487a72, subsubfolder1 };{e9ae5700-d969-430a-b4f2-445e3091a901,subsubfolder2 }                                                                                                               |
| openEHR-EHR-FOLDER.generic.v1,'root1'      | openEHR-EHR-FOLDER.episode_of_care.v1,'subsubfolder1' | {0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3 , subsubfolder1}; {13661fe2-1e16-4c75-a10d-9b8040487a72, subsubfolder1 }                                                                                                             |



## Multiple EHR


### Select by Folder archetype id and name

1. Create ehr and save {ehr_id1}
2. Create directory `folder_complex_hierarchy`
3. Create ehr and save {ehr_id2}
4. Create directory `folder_complex_hierarchy2`
5. Run Query `SELECT e/ehr_id/value, f/uid/value FROM EHR e CONTAINS FOLDER f[openEHR-EHR-FOLDER.episode_of_care.v1,'subsubfolder1']`
6. Check result is in any order

| e/uid/value | f/uid/value                          |
|-------------|--------------------------------------|
| {ehr_id1}   | 0cc504b1-4d6d-4cd5-81d9-0ef1b870edb3 |
| {ehr_id1}   | 13661fe2-1e16-4c75-a10d-9b8040487a72 |
| {ehr_id2}   | 04689137-90bf-456b-8afc-7c5774843919 |
| {ehr_id2}   | 59bbd141-c51b-435a-9cbb-85d953ebfcd3 |


### Select by Folder and EHR id

1. Create ehr and save {ehr_id1}
2. Create directory `folder_complex_hierarchy`
3. Create ehr and save {ehr_id2}
4. Create directory `folder_complex_hierarchy2`
5. Run Query `SELECT e/ehr_id/value, f/uid/value FROM EHR e CONTAINS FOLDER f WHERE e/ehr_id/value = {ehr_id1} AND f/uid/value = '13661fe2-1e16-4c75-a10d-9b8040487a72'`
6. Check result is in any order

| e/uid/value | f/uid/value                          |
|-------------|--------------------------------------|
| {ehr_id1}   | 13661fe2-1e16-4c75-a10d-9b8040487a72 |



## Contains COMPOSITION


### Select all

1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id1}`.
4. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id2}`.
5. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id3}`.
6. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id4}`.
7. Create directory `folder_with_compositions.json` replacing the parameters
8. Run Query `SELECT c/uid/value, f/name/value from FOLDER f contains COMPOSITION c`
9. Check result is in any order

| f/name/value     | c/uid/value |
|------------------|-------------|
| root1            | {comp_id1}  |
| root1            | {comp_id2}  |
| root1            | {comp_id3}  |
| root1            | {comp_id4}  |
| subfolder1       | {comp_id1}  |
| subfolder1       | {comp_id2}  |
| subfolder1       | {comp_id3}  |
| subfolder1       | {comp_id4}  |
| subsubfolder1    | {comp_id2}  |
| subsubfolder1    | {comp_id4}  |
| subsubfolder2    | {comp_id3}  |
| subsubsubfolder1 | {comp_id4}  |


### Select by name

1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id1}`.
4. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id2}`.
5. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id3}`.
6. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id4}`.
7. Create directory `folder_with_compositions.json` replacing the parameters
8. Run Query `SELECT c/uid/value from FOLDER f contains COMPOSITION c where f/name/value = {name}`

| {name}           | result in any order                            |
|------------------|------------------------------------------------|
| root1            | {comp_id1} ,{comp_id2} ,{comp_id3} ,{comp_id4} |
| subfolder1       | {comp_id1} ,{comp_id2} ,{comp_id3} ,{comp_id4} |
| subsubfolder1    | {comp_id2} ,{comp_id4}                         |
| subsubfolder2    | {comp_id3}                                     |
| subsubsubfolder1 | {comp_id4}                                     |


### Folder in Folder

1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id1}`.
4. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id2}`.
5. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id3}`.
6. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id4}`.
7. Create directory `folder_with_compositions.json` replacing the parameters
8. Run Query `SELECT c/uid/value FROM FOLDER f1[openEHR-EHR-FOLDER.episode_of_care.v1] CONTAINS FOLDER f2[openEHR-EHR-FOLDER.episode_of_care.v1] CONTAINS COMPOSITION c`
9. Check result is one row with `{comp_id4}`.


### Multiple EHR Select by Folder name

1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr save {ehr_id1}
3. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id1}`.
4. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id2}`.
5. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id3}`.
6. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id4}`.
7. Create directory `folder_with_compositions.json` replacing the parameters
8. Create composition  `conformance_ehrbase.de.v0_max.json`
9. Create ehr save {ehr_id2}
10. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id1b}`.
11. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id2b}`.
12. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id3b}`.
13. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id4b}`.
14. Create directory `folder_with_compositions.json` replacing the parameters
15. Create composition  `conformance_ehrbase.de.v0_max.json`
16. Run Query `SELECT e/ehr_id/value, c/uid/value FROM EHR e CONTAINS FOLDER f CONTAINS COMPOSITION c WHERE f/name/value = 'subsubsubfolder1'`
17. Result in any order

| e/ehr_id/value | c/uid/value |
|----------------|-------------|
| {ehr_id1}      | {comp_id4}  |
| {ehr_id2}      | {comp_id4b} |


### Multiple EHR and Compositions Select by IDs

1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr save {ehr_id1}
3. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id1}`.
4. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id2}`.
5. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id3}`.
6. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id4}`.
7. Create directory `folder_with_compositions.json` replacing the parameters
8. Create composition  `conformance_ehrbase.de.v0_max.json`
9. Create ehr save {ehr_id2}
10. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id1b}`.
11. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id2b}`.
12. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id3b}`.
13. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id4b}`.
14. Create directory `folder_with_compositions.json` replacing the parameters
15. Create composition  `conformance_ehrbase.de.v0_max.json`
16. Run Query `SELECT e/ehr_id/value, f/uid/value, c/uid/value FROM EHR e CONTAINS FOLDER f CONTAINS COMPOSITION c WHERE e/ehr_id/value = {ehr_id2} AND f/uid/value = '70939d97-8add-4419-b27c-516e64b1c744' AND c/uid/value = {comp_id4b}`
7Result in any order

| e/ehr_id/value | f/uid/value                           | c/uid/value |
|----------------|---------------------------------------|-------------|
| {ehr_id2}      | 70939d97-8add-4419-b27c-516e64b1c744  | {comp_id4b} |


### Multi Compositions in a Folder

1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id1}`.
4. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id2}`.
5. Create directory `folder_multi_compositions` replacing the parameters
6. Create composition  `conformance_ehrbase.de.v0_max.json`
7. Run Query `SELECT c/uid/value FROM FOLDER f CONTAINS COMPOSITION c`
8. Result is in any order `{comp_id1}`,`{comp_id2}`



## Select paths in FOLDER

### Select paths in Folder

1. Create ehr
2. Create directory `folder_details`
3. Run Query `SELECT {path} from FOLDER f`

| {path}                                        | result                                                                         |
|-----------------------------------------------|--------------------------------------------------------------------------------|
| f                                             | 2 rows with json of type folder                                                |
| f/items/id/value                              | null,7c0a9df0-564f-4f34-8e65-92586c64ef56,c68131a3-72da-41fe-8d11-c4fccfd2d2d0 |
| f/items                                       | null,json arry with 2 elements                                                 |
| f/details                                     | 2 rows with json of type ITEM_TREE                                             |
| f/details[at0003]/items[at0004]/value/value   | value1,value2                                                                  |


### Find items

1. Create ehr
2. Upload `conformance_ehrbase.de.v0.opt` if not exist
3. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id1}`.
4. Create composition  `conformance_ehrbase.de.v0_max.json` and save `{comp_id2}`.
5. Create directory `folder_multi_compositions` replacing the parameters
6. Create composition  `conformance_ehrbase.de.v0_max.json`
7. Run Query `SELECT f/uid/value, f/name/value, f/archetype_node_id, f/items/id/value from FOLDER f`
8. check result is in any order

| f/uid/value                          | f/name/value | f/archetype_node_id           | f/items/id/value |
|--------------------------------------|--------------|-------------------------------|------------------|
| 10e952ca-a5b2-4f24-8d37-59240fd37020 | root1        | openEHR-EHR-FOLDER.generic.v1 | {comp_id1}       |
| 10e952ca-a5b2-4f24-8d37-59240fd37020 | root1        | openEHR-EHR-FOLDER.generic.v1 | {comp_id2}       |


### Find by specific item id - VERSIONED_COMPOSITION - HIER_OBJECT_ID

1. Create ehr
2. Create directory `folder_details`
3. Run Query `SELECT f/uid/value, f/name/value, f/items/id/value FROM FOLDER f WHERE f/items/id/value = '7c0a9df0-564f-4f34-8e65-92586c64ef56'`
4. Check result to be the same as below

| f/uid/value                          | f/name/value | f/items/id/value                     |
|--------------------------------------|--------------|--------------------------------------|
| d936409e-901f-4994-8d33-ed104d460151 | subfolder1   | 7c0a9df0-564f-4f34-8e65-92586c64ef56 |