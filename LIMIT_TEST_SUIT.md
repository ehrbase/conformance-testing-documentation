# LIMIT
## LIMIT + OFFSET 
### Without order by 
1. Upload `conformance_ehrbase.de.v0.opt` if not exist
2. Create ehr
3. Create 4 times composition  `conformance_ehrbase.de.v0_max.json`
4. Run Query 'Select `SELECT c FROM COMPOSITION C LIMIT {limit} OFFSET {offset}`

| limit | offset | result count |
|-------|--------|--------------|
| 10    | NULL   | 4            |
| 4     | NULL   | 4            |
| 3     | NULL   | 3            |
| 2     | NULL   | 2            |
| 1     | NULL   | 1            |
| 10    | 1      | 4            |
| 4     | 1      | 3            |
| 4     | 2      | 2            |
| 4     | 3      | 1            |
| 4     | 4      | 0            |
