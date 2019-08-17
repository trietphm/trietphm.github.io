+++
author = "Triet Pham"
title = "SQL Query for PostreSQL Workshop"
+++

## Explain Analyze
### Init data

```
CREATE SEQUENCE seq1;
CREATE SEQUENCE seq2;

CREATE TABLE rdata (
	id serial PRIMARY KEY NOT NULL,
	name VARCHAR(255),
	balance FLOAT,
	unique_val1 INTEGER,
	unique_val2 INTEGER,
	registered BOOLEAN,
	created_at DATE
);
```

```
INSERT INTO rdata(name, balance, unique_val1, unique_val2, registered, created_at)
SELECT
('{John,Harry,Bob,Alice,Wilson,Nick,Dave,dAve,DaVe}'::text[])[CEIL(random()*9)] AS name,
CEIL(random() * 10000) AS balance,
nextval('seq1')::int AS unique_val1,
nextval('seq2')::int AS unique_val2,
CEIL(random()*100) > 1 AS registered,
(now() + (CEIL(random() * 365) || ' day') :: INTERVAL)::date AS created_at
FROM generate_series(1,10000);
```

```
WITH
unique_val1s as (
    SELECT row_number() over (order by random()) rn,
           unique_val1 as new_unique_val1
    FROM rdata
),
ids AS (
    SELECT row_number() over (order by random()) rn,
           id as ref_id
    FROM rdata
)
UPDATE rdata
SET unique_val1 = new_unique_val1
FROM unique_val1s
JOIN ids
  ON unique_val1s.rn = ids.rn
WHERE id = ref_id;
```

```
CREATE INDEX ON rdata(balance);
CREATE INDEX ON rdata(unique_val1);
CREATE INDEX ON rdata(unique_val2);
VACUUM analyze rdata;
```
