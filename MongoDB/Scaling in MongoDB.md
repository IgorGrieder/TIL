# Scaling

When we create a Cluster for example in MongoDB Atlas we usually get 3 copies of
teh same data base, one `master` and two for resources. This happens because
mondo will use it to `Scale reading` from the database. The way MongoDB Atlas
scales is horizontally with more replicas of the master DB, called `Replication`.

## Reading

When getting information from the DB it will be able to use 3 DB's insdie a
cluster, making it faster to read.

## Writing

For writting is a little different, the data is written in the `master` database
and then it is replicated in the other copyes, making it not a sacalable
operation.
