What happens if you attempt to insert a row that already exists?

We already have these rows in the attendance table:

```
SELECT * FROM attendance;
```{{execute}}

Let's try adding this row:

```
INSERT INTO attendance (event_id, client_id, attend_status)
VALUES (
    (SELECT id FROM event where lower(event_dt) = '2020-01-11 17:00'::timestamptz),
    3,
    'No Show')
;
```{{execute}}

We get an error that tells us that we're violating a constraint:

>ERROR: duplicate key value violates unique constraint "attendance_pkey"
>DETAIL: Key (event_id, client_id)=(4, 3) already exists.

What we can do is tell Postgres to take a particular action 
if there is a conflict, using the ON CONFLICT clause.

>**Note:**
>ON CONFLICT is only supported in Postgres 9.5 and above.

### ON CONFLICT DO NOTHING

This tells Postgres to not do anything when there is a conflict - not even 
raise an error:

```
INSERT INTO attendance (event_id, client_id, attend_status)
VALUES (
    (SELECT id FROM event where lower(event_dt) = '2020-01-11 17:00'::timestamptz),
    3,
    'No Show')
ON CONFLICT ON CONSTRAINT attendance_pkey
DO NOTHING
;
```{{execute}}

This is the exact same conflict as the previous statement, but this 
time there is no error message. The return message just indicates that no 
rows have been inserted:

>INSERT 0 0

The second number that's returned from INSERT indicates the number of rows 
which were actually written into the table (zero in this case). The first 
number used to be `LastOid` but is now hard-coded to `InvalidOid` (0) in all 
modern versions of PostgreSQL, to avoid breaking backwards compatibility.

And to verify that nothing has actually been changed:

```
SELECT * FROM attendance;
```{{execute}}

DO NOTHING also doesn't need the conflict to be specified. In the example 
above, if we had written `ON CONFLICT DO NOTHING` and left out ON CONSTRAINT, 
the result is still the same.

### ON CONFLICT DO UPDATE

You can use DO UPDATE to update the existing row where there is a conflict (and
go ahead with the INSERT otherwise -- this is also referred to as an "upsert" in
other database systems).

Our `workshop` database also has a payment table that stores payment 
information:

```
\d payment
```{{execute}}

```
SELECT * FROM payment;
```{{execute}}

Let's try an upsert (we're only including an `id` value here for demo purposes):

```
INSERT INTO payment (id, date, type, client_id, event_id, amount, balance)
VALUES (
        1,
        '2020-01-11 16:00 PST',
        'Event',
        1,
        4,
        20.00,
        0.00)
ON CONFLICT (id) DO UPDATE
    SET date = excluded.date;

SELECT * FROM payment;
```{{execute}}

`excluded` is a special table that contains the values you attempted to insert. 
Even with the conflict, we see that the existing row's `date` has been updated 
with the new value.

You can also restrict the upsert to only rows that meet more specific 
criteria by using the WHERE predicate, like so:

```
INSERT INTO payment (id, date, type, client_id, event_id, amount, balance)
VALUES (
        1,
        '2020-01-11 13:00 PST',
        'Event',
        1,
        4,
        20.00,
        0.00)
ON CONFLICT (id) DO UPDATE
    SET date = excluded.date WHERE payment.status IS NOT NULL;

SELECT * FROM payment;
```{{execute}}

While we do have a conflict on the `id` column, the row doesn't meet the 
additional criteria so it was not changed in the end.

Note that there is an order of operations - the additional criteria is 
evaluated only _after_ the conflict is identified.
