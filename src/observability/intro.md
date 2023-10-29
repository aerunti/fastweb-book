# Observability

What is observability?

In general, observability is the extent to which you can understand the internal state or condition of a complex system based only on knowledge of its external outputs.
[ref](https://www.ibm.com/topics/observability)


# Observability for the poor man

While there are several options for those with deep pockets, here's a simpler approach for those projects that have financing constraints.

# the poor man stack - the simplest observability solution you can implement

what you need:

- a problem that deserves be observed
- a script that check regurlaly if that problem is ocurring
- a way to send a warning to someone or apply a correction automatically

## a simple case

a client have a script that insert everyday new values to a database. we need to check every day if new values were inserted. 

the path to implement is :
- do a query to check if new values were inserted today
- If there are no new values, run the insertion procedure again
- check again if the new values where inserted
- if not solved, warn someone: "Houston, we have a problem! the system did not insert today's new values ​​into the xyz table "



