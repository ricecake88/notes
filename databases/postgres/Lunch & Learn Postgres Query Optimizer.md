Postgres Query Optimizer
Friday January 20

The Bow Man YouTube if I want to read ore on it.

Picks out - a scan method, a join method, and a join order (order that matches that data)

Temp tables are not analyzed.
After ANALYZE, the Query Plan is different for 'p' is different than querying 'd' and 'k'
The scans are all different - from Seq, Bitmap, and Index Only scans.

Seq Scans - very fast with small tables.
Index Scans - uses an index. Just a big list of a column value, there is a location with that column value. Look at index and figures out which rows match the index for those that exist and it does it after looking at each index (index, page look, index, page look, etc.). So it traverses through specific pages that matches where the indexes are from.Accessing random data is much slower than sequentially on a disk.
Bitmap Index Scan - Looks at the index - going to collect data from where it needs to get where the data comes from. So it creates a list of indexes, and THEN it accesses those pages once that Bitmap exists.

Common value: sequential scan
Reasonably uncommon: bitmap scan
Very uncommon: Index only scan
Isn't in stats: optimizer will likely use an index scan
	- essentially it just presumes it doesn't exist or is rare.

Join Methods
- Nested Loops
	- Inner Sequential Scan
		- Not very efficient for big data sets. It is available to the optimizer, but it is an option
	- Inner Index Scan
* Hash Join
	* Databases tend to like this a lot, and less expensive
	* All data have to be in memory to do the matching and in memory.
* Merge Join
	* Both sides are sorted first
	* Walkds down both tables looking for matches
* Join Order
	* Your ordering of joins is just your opinion
	* The optimizer will decide which order the join is _actually_ performed in.
