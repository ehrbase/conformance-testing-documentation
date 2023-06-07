# FROM TEST SUIT
## TEST Complete return
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0.opt`
4. Run Query 'Select `SELECT c FROM COMPOSITION c`
5. Check c equal as JSON to  `conformance_ehrbase.de.v0.opt` minus uid
## TEST FROM Usable RM-types  [Usable RM-types A,B](https://vitagroup-ag.atlassian.net/wiki/spaces/PEN/pages/38216361/Architecture+-+AQL+Feature+List#Usable-RM-types)
### TEST FROM ehr
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0.opt`
4. Run Query 'Select `SELECT e/ehr_id/value FROM EHR e`
5. Check return one row with the ehrId 

### TEST FROM composition
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0.opt`
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
3. Create composition  `conformance_ehrbase.de.v0.opt` 
4. Run Query 'Select `SELECT t FROM {type} t`
  
| type           | expected result                           |
|----------------|-------------------------------------------|
| FEEDER_AUDIT   | 5 result;JSON with _type = FEEDER_AUDIT   |
| HISTORY        | one result;JSON with name/value = History |
| POINT_EVENT    | one result;JSON with _type = POINT_EVENT  |
| INTERVAL_EVENT | 2 result;JSON with _type = INTERVAL_EVENT |
| ITEM_TREE      | 14 result;JSON with _type = ITEM_TREE     |
| CLUSTER        | 3 result;JSON with _type = CLUSTER        |

  
