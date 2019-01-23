# Database Management Systems - Chapter 8 Solutions

### Exercise 8.1
Answer the following questions about data on external storage in a DBMS:
1. Why does a DBMS store data on external storage?
2. Why are I/O costs important in a DBMS?
3. What is a record id? Given a record's id, how many I/Os are needed to fetch it into
main memory?
4. What is the role of the buffer manager in a DBMS? What is the role of the disk space
manager? How do these layers interact with the file and access methods layer?

#### `Answer`
1. A DBMS stores data on external storage because the quantity of data is vast, and
must persist across program executions.
2. I/O costs are of primary important to a DBMS because these costs typically
dominate the time it takes to run most database operations. Optimizing the
amount of I/O’s for an operation can result in a substantial increase in speed in
the time it takes to run that operation.
3. A record id, or rid for short, is a unique identifier for a particular record in a set
of records. An rid has the property that we can identify the disk address of the
page containing the record by using the rid. The number of I/O’s required to read
a record, given a rid, is therefore 1 I/O.
4. In a DBMS, the buffer manager reads data from persistent storage into memory as
well as writes data from memory into persistent storage. The disk space manager
manages the available physical storage space of data for the DBMS. When the file
and access methods layer needs to process a page, it asks the buffer manager to
fetch the page and put it into memory if it is not all ready in memory. When the
files and access methods layer needs additional space to hold new records in a file,
it asks the disk space manager to allocate an additional disk page.
***
### Exercise 8.2 Answer the following questions about files and indexes:
1. What operations arc supported by the file of records abstraction?
2. What is an index on a file of records? What is a search key for an index? Why do we
need indexes?
3. What alternatives are available for the data entries in an index?
4. What is the difference between a primary index and a secondary index? What is a duplicate data entry in an index? Can a primary index contain duplicates?
5. What is the difference between a clustered index and an unclustered index? If an index contains data records as 'data entries,' can it be unclustered?
6. How many clustered indexes can you create on a file? Would you always create at least one clustered index for a file?
7. Consider Alternatives (1), (2) and (3) for 'data entries' in an index, as discussed in Section 8.2 . Are all of them suitable for secondary indexes? Explain.

#### `Answer`

***
### Exercise 8.3
Consider a relation stored as a randomly ordered file for which the only index is an unclustered index on a field called sal. If you want to retrieve all records with sal> 20, is using the index always the best alternative? Explain.
#### `Answer`
No. In this case, the index is unclustered, each qualifying data entry
could contain an rid that points to a distinct data page, leading to as many data page
I/Os as the number of data entries that match the range query. In this situation, using
index is actually worse than file scan.
***
### Exercise 8.4
Consider the instance of the Students relation shown in Figure 8.6, sorted by age: For the purposes of this question, assume that these tuples are stored in a sorted file in the order shown; the first tuple is on page 1 the second tuple is also on page 1; and so on. Each page can store up to three data records; so the fourth tuple is on page 2. Explain what the data entries in each of the following indexes contain. If the order of entries is significant, say so and explain why. If such all index cannot be constructed, say so and explain why.
1. An unclustered index on age using Alternative (1).
2. An unclustered index on age using Alternative (2).
3. An unclustered index on age using Alternative (3).
4. A clustered index on age using Alternative (1).
5. A clustered index on age using Alternative (2).
6. A clustered index on age using Alternative (3).
7. An unclustered index on gpa using Alternative (1).
8. An unclustered index on gpa using Alternative (2).
9. An unclustered index on gpa using Alternative (3).
10. A clustered index on gpa using Alternative (1).
11. A clustered index on gpa using Alternative (2).
12. A clustered index on gpa using Alternative (3).

#### `Answer`

***
### Exercise 8.5
Explain the difference between Hash indexes and B+-tree indexes. In particular, discuss how equality and range searches work, using an example.
#### `Answer`
A Hash index is constructed by using a hashing function that quickly
maps an search key value to a specific location in an array-like list of elements called
buckets. The buckets are often constructed such that there are more bucket locations
than there are possible search key values, and the hashing function is chosen so that
it is not often that two search key values hash to the same bucket. A B+-tree index
is constructed by sorting the data on the search key and maintaining a hierarchical
search data structure that directs searches to the correct page of data entries.
***
### Exercise 8.6
Fill in the I/O costs in Figure 8.7.
#### `Answer`

***
### Exercise 8.7
If you were about to create an index on a relation, what considerations would guide your choice? Discuss:
1. The choice of primary index.
2. Clustered versus unclustered indexes.
3. Hash versus tree indexes.
4. The use of a sorted file rather than a tree-based index.
5. Choice of search key for the index. What is a composite search key, and what considerations are made in choosing composite search keys? What are index-only plans, and what is the influence of potential index-only evaluation plans on the choice of search key
for an index?

#### `Answer`
1. The choice of the primary key is made based on the semantics of the data. If we
need to retrieve records based on the value of the primary key, as is likely, we
should build an index using this as the search key. If we need to retrieve records
based on the values of fields that do not constitute the primary key, we build (by
definition) a secondary index using (the combination of) these fields as the search
key.
2. A clustered index offers much better range query performance, but essentially the
same equality search performance (modulo duplicates) as an unclustered index.
Further, a clustered index is typically more expensive to maintain than an unclustered
index. Therefore, we should make an index be clustered only if range queries
are important on its search key. At most one of the indexes on a relation can be
clustered, and if range queries are anticipated on more than one combination of
fields, we have to choose the combination that is most important and make that
be the search key of the clustered index.
3. If it is likely that ranged queries are going to be performed often, then we should
use a B+-tree on the index for the relation since hash indexes cannot perform
range queries. If it is more likely that we are only going to perform equality
queries, for example the case of social security numbers, than hash indexes are
the best choice since they allow for the faster retrieval than B+-trees by 2-3 I/Os
per request.
4. First of all, both sorted files and tree-based indexes offer fast searches. Insertions
and deletions, though, are much faster for tree-based indexes than sorted files. On
78 Chapter 8
the other hand scans and range searches with many matches are much faster for
sorted files than tree-based indexes. Therefore, if we have read-only data that is
not going to be modified often, it is better to go with a sorted file, whereas if we
have data that we intend to modify often, then we should go with a tree-based
index.
5. A composite search key is a key that contains several fields. A composite search
key can support a broader range as well as increase the possibility for an indexonly
plan, but are more costly to maintain and store. An index-only plan is query
evaluation plan where we only need to access the indexes for the data records, and
not the data records themselves, in order to answer the query. Obviously, indexonly
plans are much faster than regular plans since it does not require reading of
the data records. If it is likely that we are going to performing certain operations
repeatly that only require accessing one field, for example the average value of a
field, it would be an advantage to create a search key on this field since we could
then accomplish it with an index-only plan.
***
### Exercise 8.8
Consider a delete specified using an equality condition. For each of the five file organizations, what is the cost if no record qualifies? What is the cost if the condition is not on a key?
#### `Answer`

***
### Exercise 8.9
What main conclusions can you draw from the discussion of the five basic file organizations discussed in Section 8.4? Which of the five organizations would you choose for a file where the most frequent operations are as follows?
1. Search for records based on a range of field values.
2. Perform inserts and scans, where the order of records docs not matter.
3. Search for a record based on a particular field value.

#### `Answer`
The main conclusion about the five file organizations is that all five have
their own advantages and disadvantages. No one file organization is uniformly superior
in all situations. The choice of appropriate structures for a given data set can have a
significant impact upon performance. An unordered file is best if only full file scans
are desired. A hash indexed file is best if the most common operation is an equality
selection. A sorted file is best if range selections are desired and the data is static; a
clustered B+ tree is best if range selections are important and the data is dynamic.
An unclustered B+ tree index is useful for selections over small ranges, especially if
we need to cluster on another search key to support some common query.
1. Using these fields as the search key, we would choose a sorted file organization or
a clustered B+ tree depending on whether the data is static or not.
2. Heap file would be the best fit in this situation.
3. Using this particular field as the search key, choosing a hash indexed file would
be the best.
***
### Exercise 8.10 Consider the following relation:
Emp( eid: integer, sal: integer l age: real, did: integer)
There is a clustered index on cid and an llnclustered index on age.
1. How would you use the indexes to enforce the constraint that eid is a key?
2. Give an example of an update that is definitely speeded 1lJi because of the available
indexes. (English description is sufficient.)
3. Give an example of an update that is definitely slowed down because of the indexes.
(English description is sufficient.)
4. Can you give an example of an update that is neither speeded up nor slowed down by the indexes?

#### `Answer`

***
### Exercise 8.11
Consider the following relations:  

Emp( eid: integer, ename: varchar, sal: integer, age: integer, did: integer)  
Dept(did: integer, budget: integer, floor: integer, mgr_eid: integer)  

Salaries range from $10,000 to $100,000, ages vary from 20 to 80, each department has about five employees on average, there are 10 floors, and budgets vary from $10,000 to $1 million. You can assume uniform distributions of values. For each of the following queries, which of the listed index choices would you choose to speed up the query? If your database system does not consider index-only plans (i.e., data records are always retrieved even if enough information is available in the index entry), how would
your answer change? Explain briefly.
1. Query: Print ename, age, and sal for all employees.
(a) Clustered hash index on (ename, age, sal) fields of Emp.  
(b) Unclustered hash index on (ename, age, sal) fields of Emp.  
(c) Clustered B+ tree index on (ename, age, sal) fields of Emp.  
(d) Unclustered hash index on (eid, did) fields of Emp.  
(e) No index.  

2. Query: Find the dids of departments that are on the 10th floor and have a budget of less than $15,000.
(a) Clustered hash index on the floor field of Dept.  
(b) Unclustered hash index on the floor' field of Dept.  
(c) Clustered B+ tree index on (floor, budget) fields of Dept.  
(d) Clustered B+ tree index on the budget: field of Dept.  
(e) No index.  

#### `Answer`
1. We should create an unclustered hash index on ename, age, sal fields of Emp
(b) since then we could do an index only scan. If our system does not include
index only plans then we shouldn’t create an index for this query (e). Since this
query requires us to access all the Emp records, an index won’t help us any, and
so should we access the records using a filescan.
2. We should create a clustered dense B+ tree index (c) on floor, budget fields of
Dept, since the records would be ordered on these fields then. So when executing
this query, the first record with floor = 10 must be retrieved, and then the other
records with floor = 10 can be read in order of budget. Note that this plan,
which is the best for this query, is not an index-only plan (must look up dids).
***
