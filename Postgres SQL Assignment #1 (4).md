# Postgres SQL Assignment #1 - Tennyson Lee
###### This assignment should solidify and test your knowledge with basic SELECT statements. Your answers should be written in a PDF document or plain text file, and should be numbered according to the problem numbers in this assignment. For those of you familiar with R, it is suggested that you use RMarkdown to knit a clean-looking PDF. For each problem, you are expected to write a single SQL query as your solution and on the next line copy and paste the output of that query. Only include the first FIVE lines of output for any queries that output more than five lines. For all problems, only return the information and columns requested and only use SQL commands shown in class. Every query should produce at minimum one column.

**Using the table cmspop, write a query that finds the following:**  
A.	All rows where the claim is in California and the patient is a caucasian female.  
`SELECT * FROM cmspop WHERE state = 'CA' AND race = 'white' AND sex = 'female';`

| id | dob |    dod     |  sex   | race  | state | county | alz_rel_sen | heart_fail | cancer | depression |
|-----|----|------|----|-----|------|-------|:-------:|:---------:|:--------:|:------------:|
 00131C35661B2926 | 1959-11-01 |            | female | white | CA    |     90 | f           | t          | t      | t
 001A8AC6DB47D3DB | 1937-08-01 |            | female | white | CA    |    640 | f           | f          | f      | f
 001E248F6DB5B893 | 1967-02-01 |            | female | white | CA    |    210 | f           | f          | f      | f
 00205821D74AA7D8 | 1964-05-01 |            | female | white | CA    |    660 | t           | t          | t      | f
 00247AD32AE68EFE | 1940-05-01 |            | female | white | CA    |    510 | f           | f          | f      | f

B.	The number of claims in Washington where the patient is male.  
`SELECT COUNT(*) FROM cmspop WHERE state = 'WA' AND sex = 'male';`

|count |
|-------|
|19909| 
(1 row)

C.	The claims where id contains either “000” or “34”.  
`SELECT * FROM cmspop WHERE id LIKE '%000%' OR id LIKE '%34%';`

|  id  |    dob     |    dod     |  sex   |   race   | state | county | alz_rel_sen | heart_fail | cancer | depression |
|------|--------|----|----|------|------|--------|:-------:|:-------:|:----:|:-----:|
 00013D2EFD8E45D1 | 1923-05-01 |            | male   | white    | MO    |    950 | f           | t          | f      | f
 00016F745862898F | 1943-01-01 |            | male   | white    | PA    |    230 | t           | t          | f      | f
 0001FDD721E223DC | 1936-09-01 |            | female | white    | PA    |    280 | f           | f          | f      | f
 00021CA6FF03E670 | 1941-06-01 |            | male   | hispanic | CO    |    290 | f           | f          | f      | f
 00024B3D2352D2D0 | 1936-08-01 |            | male   | white    | WI    |    590 | f           | f          | f      | f


D.	The first-born person in Florida that is deceased (you can ignore the fact that more than one person might be born on the same day).  
`SELECT * FROM cmspop WHERE state = 'FL' ORDER BY dob, dod LIMIT 1;`

| id |    dob     |    dod     |  sex   |   race   | state | county | alz_rel_sen | heart_fail | cancer | depression |
|----|-------|-------|-----|----------|-------|--------|-------|---------|-----|---------|
 AA3B49AB7C9C5294 | 1909-01-01 | 2010-05-01 | male   | white    | FL    |    340 | f           | f          | f      | f



**Using the table cmsclaims, write a query that does the following:**  
A.	Calculates the ratio of carrier reimbursement to beneficiary responsibility in descending order.  
`SELECT id, carrier_reimb, bene_resp, (carrier_reimb::float/bene_resp) AS ratio FROM cmsclaims WHERE bene_resp > 0 ORDER BY ratio DESC;`

|    id        | carrier_reimb | bene_resp | ratio |
|--------------|---------------|-----------|-------|
 5661B3E9B2CEDC6A |          1550 |        10 |   155
 D14622CE2202C0F2 |          1270 |        10 |   127
 83BCECD91296F0CD |          1160 |        10 |   116
 4ABAC98A37197785 |          1150 |        10 |   115
 BB72084FD549F885 |          1090 |        10 |   109.5


B.	Finds the amount of greatest beneficiary responsibility.  
`SELECT MAX(bene_resp) FROM cmsclaims;`

| max  |
|------|
| 3590 |
(1 row)

C.	Finds the five records with the largest ratio of beneficiary responsibility to carrier reimbursement (note: this ratio is different than in sub-problem a) where the number of HMO months is 4. Return columns with the id, hmo_mo, and the ratio.  
`SELECT id, hmo_mo, (bene_resp::float/carrier_reimb) AS ratio FROM cmsclaims WHERE carrier_reimb > 0 AND hmo_mo = 4 ORDER BY ratio DESC LIMIT 5;`

|        id        | hmo_mo | ratio |
|------------------|--------|-------|
 1FD6A4F354C9F70C |      4 |    11
 6735A9DE2BB3AB96 |      4 |     6
 0857404ECFB3F162 |      4 |     6
 F6CA27664F2B6285 |      4 |     4
 928A8BBDF72F1322 |      4 |     3.5
(5 rows)

D.	Counts the number of unique claim IDs for which the beneficiary was not financially responsible. Assume claim IDs in the data can contain duplicates.  
`SELECT COUNT(DISTINCT id) FROM cmsclaims WHERE bene_resp = 0;`

| count  |
|--------|
| 604130 |
(1 row)

E.	Finds the average difference between carrier reimbursement and beneficiary responsibility.  
`SELECT AVG(carrier_reimb - bene_resp) AS avg_diff FROM cmsclaims;`

|       avg_diff       |
|----------------------|
| 608.4206540026198418 |
(1 row)

