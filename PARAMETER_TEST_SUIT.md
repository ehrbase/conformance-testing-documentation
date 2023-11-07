# PARAMETER
## Parameter in where

1. Upload `conformance-ehrbase.de.v0` if not exist
2. Create ehr
3. Create composition  `conformance_ehrbase.de.v0_where.json`
4. Run the following Query with parameters
5. ```
   {
   "q": "SELECT {path} FROM COMPOSITION C CONTAINS OBSERVATION o WHERE {path} = &param",
   "query_parameters": {
      "param": "{value}",
    }
   }
   ```


| path                                                                     | {value}               | result              |
|--------------------------------------------------------------------------|-----------------------|---------------------|
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | 'Lorem ipsum'         | "Lorem ipsum"       |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0004]/value/value     | 'Lorem ipsum 2'       | "Lorem ipsum 2"     |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude | 20                    | 20                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0008]/value/magnitude | 25                    | 25                  |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value/value     | '2022-02-03T04:05:06' | 2022-02-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0011]/value/value     | '2024-02-03T04:05:06' | 2024-02-03T04:05:06 |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0017]/value/value     | true                  | true                |
| o/data[at0001]/events[at0002]/data[at0003]/items[at0017]/value/value     | false                 | false               |
   