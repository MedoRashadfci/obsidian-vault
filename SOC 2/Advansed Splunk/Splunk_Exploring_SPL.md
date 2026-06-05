### Search & Reporting

Splunk's **Search & Reporting App** is the default interface used for searching and analyzing data on the Splunk home page. It has various functionalities that assist analysts in improving the search experience.

Upon accessing the app, we discover several key functionalities.

1. **Search Head**: Where analysts put the queies to filter or aggregate log data
2. **Time Picker**: Provides multiple options to select the timeframe of your search
3. **Search History**: Saves Splunk's search queries that have previously been used
4. **Data Summary**: Provides a summary of the hosts, sources, and sourcetypes available

Take a look through the search history in your Splunk instance and begin by answering the first question.

![A screenshot of the Splunk Search & Reporting App highlighting the search head, time duration picker, search history, and data summary.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1761637957195.svg)

## Your First Search

In this room, we will be working with the `windowslogs` index, where index is like a Splunk database or container for organizing the data. You can proceed by submitting your first query using the search function and setting the time range to `All time`.

![A screenshot of the query index=windowslogs using the Splunk search head. The query and All time range are highlighted.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1761982351687.svg)

|   |
|---|
|**Note**: Both `index=windowslogs` and `index = windowslogs` are valid syntax in Splunk.|

## Fields Sidebar

The Field Sidebar can be found on the left panel of Splunk search. This sidebar features two sections: one displays selected fields, and the other highlights interesting fields. It also provides quick results, including top values and raw values for each field.

1. **Selected Fields**: The default extracted fields. You can select other fields by clicking them and toggling `Selected`
2. **Interesting Fields**: Pulls all the interesting fields it finds and displays them in the left panel to further explore
3. **Numeric Fields** `#`: This symbol represents fields that contain numerical values
4. **Alpha-numeric Fields** `α`: The alpha symbol represents fields that contain strings (text values)
5. **Count**: The number of events containing the listed field
6. **More available fields**: If more fields are available, they can be accessed and selected here

![A screenshot of the Splunk fields sidebar highlighting selected fields, interesting fields, numeric value fields, alpha-numeric value fields, field count, and more available fields. The selected: yes/no option for the AccountName field is also highlighted.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1761637958326.svg)

Answer the questions below

Submit your first query for All Time: `index=windowslogs`.  
How many total events do you see?

12256

After you submit your first query, look in the Fields sidebar.  
Which `SourceIP` has recorded the most amount of events?

172.90.12.11

How many events appear on 04/15/2022 from 08:05 AM to 08:06 AM?

134

-----
------

### Search Operators

Splunk's **Search Processing Language** (SPL) is behind every search in Splunk. It combines commands, functions, and operators that allow you to filter, transform, and analyze data from your ingested logs. In essence, SPL lets you search through massive amounts of data, apply filters to narrow down results, and format the output. Let's see how to use SPL.

## Free Text Search

The simplest way to use SPL is to use free-text search, such as in the query below:

```c
index=windowslogs alice
```

The query will search for all events containing the **alice** keyword (case-insensitive). If you don't know the field names or just want to run a quick hunt for a unique keyword, free-text searches are your best choice. However, to run more complex searches, you'd need to use search operators and work with parsed **fields** and their **values**.

## Search Operators

Splunk [operators(opens in new tab)](https://help.splunk.com/en/splunk-enterprise/search/search-manual/10.0/expressions-and-predicates/predicate-expressions) are the building blocks used to construct any search query. These operators are used to filter, remove, and refine your search results based on the specified criteria. Below, we will cover relational, logical, and wildcard operators. Note that for the filters to work, the events must be parsed into fields - the next rooms will explain the process in more detail.

**Relational Operators**

These operators are used to compare two expressions. They determine the relationship between the expressions, such as whether one is equal to, not equal to, greater than, or less than the other. Let's check out some examples below.

|Operator|Example|Explanation|
|---|---|---|
|Equals `=`|`UserName=Mark`|Search for all events in which the field name `UserName` is equal to `Mark`|
|Not Equal To `!=`|`UserName!=Mark`|Search for all events in which the field name `UserName` is not equal to `Mark`|
|Less Than `<`|`Age<10`|The field `Age` has a value of less than `10`|
|Less Than or Equal To `<=`|`Age<=10`|The field `Age` has a value of less than or equal to `10`|
|Greater Than `>`|`Outbound_Traffic>50`|The `Outbound_Traffic` field value is greater than `50`|
|Greater Than or Equal To `>=`|`Outbound_Traffic>=50`|The `Outbound_Traffic` field value is greater than or equal to `50`|

Let's get hands-on and use the **!=** relational operator to locate all event logs in our index where the field **AccountName** is not equal to **System**. Start with the query below and don't forget to set your time range back to **All time**!

```c
index=windowslogs AccountName!=SYSTEM
```

In the screenshot below and in your Splunk instance, you can see that we have successfully filtered for all events that do not include the **AccountName** field value of **SYSTEM**.

![A screenshot of a Splunk query searching for the AccountName field that is not equal to SYSTEM. The AccountName field is highlighted, as well as the field values.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1761983277759.svg)

**Logical Operators**

Splunk supports the following logical operators, which can be used to connect or modify conditions and operate on Boolean values (true/false). 

|Operator|Example|Explanation|
|---|---|---|
|`NOT`|`NOT UserName=*`|Returns events where `UserName` field does not exist. Don't confuse it with `!=` operator|
|`AND`|`UserName=David AND IPAddress=10.10.10.10`|Returns all events in which the `UserName` field is equal to `David` and the `IPAddress` field is equal to`10.10.10.10`|
|`OR`|`UserName=David OR UserName=John`|Returns all events in which the `UserName` field is equal to `David` or `John`|
|`IN`|`UserName IN(David, John)`|A more convenient alternative to the `OR` keyword, especially for long lists.|

Let's get some more practice by appending the previous query to search for events that have the **AccountName** field **James**. With this query, you are telling Splunk to filter out the **SYSTEM** account name, and from the results, only see events from the account name **James**:

```c
// AND operator is implied, so both queries are valid
index=windowslogs AccountName!=SYSTEM AND AccountName=James
index=windowslogs AccountName!=SYSTEM AccountName=James
```

![A screenshot of a Splunk query searching for the AccountName field not equal to SYSTEM and AccountName field equal to James. The query and searched AccountName James are both highlighted.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1761983277849.svg)

**Wildcards and CIDR Search**

Splunk supports the use of wildcards and CIDR search for IP addresses to search ffor a partial or IP subnet match. For example:

|Symbol|Example|Explanation|
|---|---|---|
|`*`|`status=*fail*`|This will return all events that have `status` field set to `failed`, `failure`, `appfail`, etc.|
|`*`|`DestinationIp=172.*`|This will return all events that contain values like `DestinationIp=172.90.0.0.1` or `DestinationIp=172.18.5.22`|
|`N/A`|`DestinationIp=172.18.0.0/16`|This will return all events where `DestinationIp` field is within the `172.18.0.0/16` subnet|

![A screenshot of a Splunk query utilizing the wildcard symbol * to search for the field DestinationIP that begins with 172.. The query, DestinationIP field, and field values are highlighted.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1761983277642.svg)

## Order of Evaluation

**Quotes**

In Splunk, quotation marks `""` are used to define exact phrases or strings. You can wrap text in quotes, and Splunk will treat it as a single value. Quotes can also be used to escape search operators. For example:

- `index=windowslogs failed login`: Search for events with **failed** and **login** keywords, in any order
- `index=windowslogs "failed login"`: Search for the exact phrase "**failed login**", word order matters
- `index=windowslogs "TO BE OR NOT TO BE"`: Search for the exact phrase containing **NOT** and **OR**

**Parentheses**

You can utilize parentheses in Splunk to help group conditions together and control how the search is applied. Since **OR** operator takes precedence over **AND**, parentheses can help set the correct order of conditions. For example, imagine you want to search for events containing **alice** and **bob** together, or **charlie** alone:

|Your Search|How Splunk Evaluates It|
|---|---|
|`index=windowslogs alice AND bob OR charlie`  <br>  <br>(Implicit search, no parentheses)|`index=windowslogs alice AND (bob OR charlie)`  <br>  <br>(Mistake! Splunk evaluated OR before AND)|
|`index=windowslogs (alice AND bob) OR charlie`  <br>  <br>(Explicit search with parentheses)|`index=windowslogs (alice AND bob) OR charlie`  <br>  <br>(Correct! The results match your requirements)|

Answer the questions below

How many events in the `windowslogs` index have an `EventID` field value equal to `4624`?

26

How many events are observed with the `DestinationIp = 172.18.39.6` and `DestinationPort = 135`?

4

Use the query `index=windowslogs Hostname=Salena.Adam DestinationIp=172.18.38.5`  
Which `SourceIp` returns the highest count?

172.90.12.11

How many events are returned when you search the term `cyber*`?

12256

Which operator is given the lowest priority in Splunk searches?

AND


-----
----
----
----
### Filtering Results

Your network may generate thousands of logs every minute, all of which are ingested into your SIEM solution. Searching for anomalies without filters can quickly become overwhelming. Splunk’s Search Processing Language (SPL) enables analysts to apply filters using search commands to narrow down search results and focus only on the most relevant events.

In Splunk, commands are linked together using a pipe symbol `|`. Each pipe passes the output of one command into the next, allowing you to refine your results step by step. Let’s look at some useful commands that help filter and organize search results.

## Useful Filtering Commands

**Fields**

The [fields (opens in new tab)](https://help.splunk.com/en/splunk-enterprise/spl-search-reference/9.0/search-commands/fields)command is used to include or exclude specific fields from your search results. To exclude a field, use a minus sign `-` before the field name. The plus sign `+` can be used to include a field explicitly, but it isn’t required. By default, `fields` includes any fields listed after the command. Let’s try it out in our Splunk instance by highlighting the following fields. This makes it easy to see how useful the `fields` command can be when working with logs that contain hundreds of available fields.

index=windowslogs | fields host User SourceIp

![A screenshot of a Splunk query using the fields command to highlight the host, User, and SourceIp fields. The query and selected fields are highlighted.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1761983835185.svg)

**Dedup**

The [dedup(opens in new tab)](https://help.splunk.com/en/splunk-enterprise/spl-search-reference/9.0/search-commands/dedup) command removes duplicate values from your search results. For example, if our logs contain seven distinct IP addresses in the `SourceIp` field, the results will return seven events, one for each unique IP. The command is useful for subsearches and for cleaning identical events (e.g., Microsoft 365 often sends 50 events for a single activity).

```c
index=windowslogs
| fields EventID User Image Hostname SourceIp
| dedup SourceIp
```

![A screenshot of a Splunk query using the fields and dedup commands to search for the listed fields. The query and total event count are both highlighted.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1761984385617.svg)

**Rename**

The [rename(opens in new tab)](https://help.splunk.com/en/splunk-enterprise/spl-search-reference/9.0/search-commands/rename) command allows you to change the name of a field in your search results. This can help improve the readability of your search results, especially if the original fields are too long or not suitable for showing them in the screenshots in formal SOC reports.

```c
index = windowslogs
| fields EventID User Image Hostname SourceIp
| rename User as Employee
```

The command is also useful to flatten JSON or XML subfields. For example, for a JSON log entry like `{"request": {"path": "/admin", "ip": "10.0.0.2"}}`, Splunk will create two fields: `request.path` and `request.ip`. If you don't want to type the prefix every time, consider removing it like in the example below:

```c
index=jsondata
| rename request.* as * // request.path -> path; request.ip -> ip
```

![A screenshot of a Splunk query using the fields and rename command to rename the User field to Employee. The query, selected fields, and newly renamed field values are highlighted.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1761984383616.svg)

**Regex**

The [regex(opens in new tab)](https://help.splunk.com/en/splunk-enterprise/spl-search-reference/9.0/search-commands/regex) command allows you to filter search results using regular expressions, which match specific text patterns in field values. This is useful when you need to find events that follow a specific format rather than an exact keyword. Splunk regular expressions are PCRE (Perl Compatible Regular Expressions) and use the PCRE C library.

index = windowslogs | regex Image = "\.exe$"

The query above applies a regular expression to the `Image` field, returning only events where the field value ends with `.exe`. The `$` symbol specifies that the match must occur at the end of the string. That was the simplest example, but regex is irreplaceable for complex searches, especially on custom or poorly-parsed data sources.

![A screenshot of a Splunk query using the regex command to search for field values ending in .exe in the Image field. The query, Image field, and field values are highlighted.](https://tryhackme-images.s3.amazonaws.com/user-uploads/616945d482ef350052080da1/room-content/616945d482ef350052080da1-1761984387091.svg)

Answer the questions below

Use the `fields` command to highlight `Domain`, `SourceProcessId`, and `TargetProcessId`.  
Which `SourceProcessId` has the highest value?

9496

Try out this query `index=windowslogs | regex TargetObject="Manager$"`.  
Which `TargetObject` field value contains the highest number of results?

HKLM\SOFTWARE\Microsoft\SecurityManager

----
---
---
---
