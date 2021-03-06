## Running ceif
ceif is a command-line program controlled by arguments, basic syntax is:

    ceif [OPTION]...

Input data is assumed to be comma separated values. Different separator can be given by option -f.
### Options

| Option | Purpose  |
|:----|----|
| -h&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Display help and exit|
| -d&nbsp;INTEGER | Number of decimals when printing and saving dimension values. Default is 6|
| -V | Display version and exit|
| -I&nbsp;LIST | LIST is a comma separated list of field numbers (first = 1) which should be ignored when reading the input file. Ranges can be given with dash (e.g. 2-9). Default is to read all fields|
| -U&nbsp;LIST | LIST is a comma separated list of field numbers (first = 1) which should be processed when reading the input file. Ranges can be given with dash (e.g. 2-9). This can be used with -I when there are lot of unused fields|
| -X&nbsp;LIST | LIST is a comma separated list of field numbers (first = 1) which should be as text fields when reading the input file. Ranges can be given with dash (e.g. 2-9). A hash value from range 0-32770 is generated using field string value. Note that this is not collision free. Probably this should be used only for simple classifications like "yes/no" or "Male/Female/Unknown". Note also that "Yes" and "yes" will produce different value|
| -t&nbsp;INTEGER&nbsp;&nbsp; | Number of trees to use. Default is 100|
| -s&nbsp;INTEGER | Number of samples for each tree. Default is 256|
| -f&nbsp;CHAR | Field separator for input files|
| -l&nbsp;FILE | File to be used in algorithm training| 
| -a&nbsp;FILE | File to analyse|
| -c&nbsp;FILE | File to categorize|
| -p&nbsp;STRING | Printf style format to print anomaly data or categorized data. See printing directives below|
| -o&nbsp;FILE | Print output to FILE. Default is to use stdout|
| -w&nbsp;FILE | Write forest data to FILE. Typically result of analysing data using file with option -l. Data can be later read with option -r|
| -O&nbsp;FLOAT| Outlier score for anomaly detection. Data with higher or equal score is considered as an anomaly and printed with format given by option -p. Use values 0.0 - 1.0.  Also option argument "auto" can be given. Then the outlier score is determined by the score from sample having maximum score. This process can be adjusted with rc-file variable AUTO\_SCORE\_FACTOR, see section user rc-file for more details |
| -r&nbsp;FILE | Read forest data from file. File should have been written earlier with option -w|
| -C&nbsp;LIST | List of field numbers to be used as a category field. Default is not to use category field. Field values are separated by colon to form a category string|
| -L&nbsp;LIST | List of field numbers to be used as a label field. Default is not to use label field. Field values are separated by colon to form a label string|
| -F&nbsp;REGEXP | Filter categories using regular expression. Forests having category string matching REGEXP are not used in analysis or categorization. Several options can be given. If REGEXP is preceded by "-v " then matching is inverted|
| -H | Input data contains a header line which is ignored. Default is to read all lines|
| -S | Set locale to local locale. Default is use locale "C"|
| -R&nbsp;FLOAT| Interception point ***p*** range extension factor. Higher value means that starting interception points are selected more away from sample data points and maximum tree height is larger|
| -T&nbsp;FLOAT| Generate test data. Test data is generated using sample set min/max values and test data point interval given by option -i (default is 256). Test data range can be enlarged by FLOAT. E.g. value 1.0 doubles the test data range. After test data is printed max 10240 sample data points are printed with score 0|
| -i&nbsp;INTEGER| Test data point interval. Larger value means more dense test data point set|
| -u&nbsp;INTEGER| Accept only unique samples when sampling input data. INTEGER is value between 0..100 (default is 10). This is the percentage of input data rows to be checked for uniqueness. Value 100 can be used if every accepted sample data should be unique|
| -m&nbsp;STRING| Printf format for printing float values for sample and sample average values. Default is "%.*f"|
| -e&nbsp;CHAR| Value separator when printing sample, sample average and analysed data values. Default is comma|
| -M&nbsp;STRING | Print category value, average values or last update time of forests which have not used in analysis. Optional printf format STRING is used in printing|
| -D&nbsp;INTEGER | Before saving the forest data to file delete forests which have not been updated INTEGER (seconds) ago. If INTEGER is followed by a letter from set Y,M,D or m the INTEGER is consired to be years, months, days or minutes.|
| -N&nbsp;STRING | Print input values which are not assosiated with any categories. This can be used for printing "new" category values. Optional printf format STRING is used in printing|
| -A | Instead taking samples as they are, aggregate new samples values for each forest. Only one new aggregated sample for each forest is added for each usage of -l option|
| -W | Scale dimensions attributes. All attribute ranges are scaled to largest attribute range. Use this if there is even minor difference between attribute scales|
| -q | Print forest information in human readable form and exit|
| -y | Print forest information ascii density map|
| -yy | Print forest information ascii density map with common sample scale for all forests|
| -E | Print samples with sample score|
| -k | Remove the sample having maximun sample score for each non filtered forest. If option is given several times, then several samples are removed. This can be used to remove outliers from samples. Modified sample set can be saved with option -w|


If FILE is "-" then standard input or output is read or written.

#### Printing directives

| Directive | Meaning |
|----|----|
| %r | Current input file row number|
| %s | Anomaly score |
| %c | Category values separated by semicolon|
| %l | Label values separated by dash|
| %d | Separated list of dimension values|
| %a | Separated list of dimension average values|
| %v | Current input row values|
| %x | Outlier score value in RGB values. Presented as hex value (.e.g 127F77). Note that value zero is printed as black|
| %C | Found category values separated by semicolon when categorizing data|
| %t | Time when category has been last updated. In human readable form using current locale|
| %: | Category value separator|
| %. | Label value separator|
| %% | Percent sign|

Value separator for d,a and v can be given by option -e.

### User rc-file
Some defaults can be read from user specific rc-file ~/.ceifrc. File has variable-value pairs separated by whitespace. Comments start with #. 
These values can be overriden by command options and saved forest data (read using option -r).

Following variables are supported:

| Variable | Meaning | default value |
|----|----|----|
|AUTO\_SCORE\_FACTOR|When calculating the auto score (option -Oauto) the sample set can be expanded before finding the score for each sample. This value adjusts how much the sample set is expanded. More larger value causes more higher auto score. Negative values can be used too, the auto score will be lower than natural maximum sample score|5|
|SAMPLES|Number of samples taken for each forest, same affect as option -s|256|
|TREES|Number of trees for each forest, same affect as option -t|100|
|DECIMALS|Number of decimals used when saving forest data. Affects also printing of sample values (option -d)|6|
|PRANGE\_EXTENSION\_FACTOR|Interception point ***p*** range extension factor (option -R)|1|
|AUTO\_SCORE|Calculate auto score, 1 = yes, 0 = no|0|
|AUTO\_WEIGTH|Scale sample values before analysing the forest, 1 = yes, 0 = no|0|
|CATEGORY\_SEPARATOR|Char to be used as a separator when concatenating category fields|;|
|LABEL\_SEPARATOR|Char to be used as a separator when concatenating label fields|-|

Example of rc-file:

    # My default values
    AUTO_SCORE_FACTOR 5
    AUTO_SCORE 1
    TREES 200
    CATEGORY_SEPARATOR +
    LABEL_SEPARATOR   .

    #End of file

### Examples

#### Learn and analyze same field
Learn and analyse file data.csv, use fields 1-3 (by ignoring fields 4-100) for analysis and print anomalies having score 0.6 or greater.

    ceif -l data.csv -a data.csv -R30 -I4-100  -O0.6

#### Learn and write forest data to file data.f

    ceif -l data.csv -w data.f -R30 -I4-100 -O0.6

#### Read forest data from data.f and analyse data.csv
Note that parameters like -R and -O are saved to forest file and need not be given again.  

    ceif -r data.f -a data.csv

####  Read forest data from data.f and add more samples from data2.csv 
Note that -w must given in order to save enhanced forest data.

    ceif -r data.f -l data2.csv -w data.f

#### Learn and write forest data with category
Use field number 5 as category field

    ceif -l data.csv -w data.f -R30 -I4-100 -O0.6 -C 5

#### categorize data from data.csv using forest data from previous example
Print analyzed category value (%C) and select fields as comma separated list (%v) for each input row from data.csv.

    ceif -r data.f -c data.csv -p "%C %d"

#### Generate test data set using forest data file
Generate data set around sample data points by enlarging the area with factor 1. Every dimension value range consists of 512 test values. 
Test data sample values and outlier score value in RGB value separated by semicolon are printed to file plot_data.csv.

    ceif -r data.f -T1 -i512 -e";" -p"%d;0x%x" -o plot_data.csv

#### Data value aggregation
If e.g. daily or hourly summaries should be analyzed then ceif should be called with -A option and each forest should have a daily or hourly aggregation key. 
Example file of hourly samples of internet traffic:

    $ cat traffic.csv
    Hour,Type,Inbytes,Outbutes
    12,In,123,1444
    12,Out,423,1644
    13,In,123,1444
    13,In,823,44
    14,In,13,1414
    14,In,9123,1444
    14,In,123,1443
    15,Out,12423,1644
    16,Out,423,16
    16,Out,493,1044
    17,Out,433,1644
    18,Out,493,1644

Making Hourly and traffic direction summaries, first two fields are category keys (-C1-2):

    ceif -l traffic.csv -C1-2 -H -A -R10 -n -d0 -w traffic.ceif

Forest file contents below, for each hour and traffic type the number of bytes are summarized:

    $ cat traffic.ceif
    G;2;"";"%s %v";100;256;"1-2";",";1;0.750000;10.000000;"";"";8;"";0;0;"";",";1;1
    F;"12:In";0.000000;0;1;1582453164
    S;123|1444
    F;"12:Out";0.000000;0;1;1582453164
    S;423|1644
    F;"13:In";0.000000;0;1;1582453164
    S;946|1488
    F;"14:In";0.000000;0;1;1582453164
    S;9259|4301
    F;"15:Out";0.000000;0;1;1582453164
    S;12423|1644
    F;"16:Out";0.000000;0;1;1582453164
    S;916|1060
    F;"17:Out";0.000000;0;1;1582453164
    S;433|1644
    F;"18:Out";0.000000;0;1;1582453164
    S;493|1644

And when the same is done next day and so on, a new sample is added for each forest (Hour and traffic direction combination).
After enough samples are collected a daily traffic can be analyzed for anomalies using -a option.

