Original by <https://github.com/luismartingarcia>

## INTRODUCTION

Protocol is a simple command-line tool that serves two purposes:
- Provide a simple way for engineers to have a look at standard network protocol headers, directly from the command-line, without having to google for the relevant RFC or for ugly header image diagrams.

- Provide a way for researchers and engineers to quickly generate ASCII RFC-like header diagrams for their own custom protocols.

## Installing protocol

The latest version of protocol is available via GitHub. It can be downloaded directly from the following website clicking on the button "Download ZIP".

Clone the repo (either this fork, other or https://github.com/luismartingarcia/protocol)

`git clone https://github.com/luismartingarcia/protocol.git`

Protocol can be installed by running the included setup.py script as follows:

`setup.py install`

Note that in order to install Protocol, root or administrative privileges are required. On GNU/Linux systems, it can be installed as follows:

`sudo ./setup.py install`

## Running protocol

Once installed, Protocol can be run from the command line. The syntax is the following:

`protocol <EXISTING_PROTOCOL_NAME(S)>`

alternatively

`protocol <CUSTOM_PROTOCOL_SPECIFICATION(S)>`

You have a list of predefined available protocols under `/specs.py`.

Here are a few examples:

```bash
protocol ~ protocol tcp

 0                   1                   2                   3  
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |        Destination Port       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Sequence Number                        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                     Acknowledgment Number                     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
| Offset|  Res. |     Flags     |             Window            |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Checksum           |         Urgent Pointer        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                    Options                    |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

```

```bash
protocol ~ protocol coap udp ipv6
0                   1                   2                   3  
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|ver| T |  TKL  |      Code     |           Message ID          |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                      Token in TKL bytes...                    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                        Options if any...                      |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|1 1 1 1 1 1 1 1|               Payload if Any ...              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

 0                   1                   2                   3  
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Source Port          |        Destination Port       |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|             Length            |            Checksum           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

 0                   1                   2                   3  
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version| Traffic Class |               Flow Label              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Payload Length        |  Next Header  |   Hop Limit   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                         Source Address                        +
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                                                               |
+                                                               +
|                                                               |
+                       Destination Address                     +
|                                                               |
+                                                               +
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

```

Protocol can represent any arbitrary protocol headers by using the  syntax:

`<LIST_OF_FIELDS>[?OPTIONS]`

Where [LIST_OF_FIELDS] is a comma-separated list of protocol field names and lengths (expressed in bits), and [?OPTIONS] is an optional part that allows users to specify format modifiers for the ASCII header. 

Note that if any of the field names contains spaces, the protocol specification must be enclosed in double quotes.

Here are is an example:

``` bash
protocol ~ protocol "Source:16,TTL:8,Reserved:40"
 0                   1                   2                   3  
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|             Source            |      TTL      |               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+               +
|                            Reserved                           |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

## Options

Users may optionally pass formatting options to control the way the ASCII header is generated.
The existing options are:

``` bash
protocol ~ protocol -h  
-b, --bits <n>      : Number of bits per line
-f, --file          : Read specs from a text file
-h, --help          : Displays this help information
-n, --no-numbers    : Do not print bit numbers on top of the header
-V, --version       : Displays current version
--evenchar  <char>  : Character for the even positions of horizontal table borders
--oddchar   <char>  : Character for the odd positions of horizontal table borders
--startchar <char>  : Character that starts horizontal table borders
--endchar   <char>  : Character that ends horizontal table borders
--sepchar   <char>  : Character that separates protocol fields
```

The following diagram shows the character modifiers described above.

```txt
     Bit counts                                                     sepchar(|)
       ^   ^                                                              |
       |   |                                                              |
       |   |                                                              |
       0   |               1                   2                   3      |
       0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1    |
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+   |
      |             Source            |      TTL      |    Reserved   | <-+
      +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
      ^              ^ ^ ^              ^ ^ ^                         ^
      |              | | |              | | |                         |
   startchar(+)     evenchar( -)       oddchar(+)                endchar(+)
      |<------------------------------------------------------------->|
                                 bits(32)
```

Here are are few examples:

```bash
protocol "Field4:4,Field4:4,Field8:8,Field16:16,Field32:32,Field64:64?\
     bits=16,numbers=y,startchar=*,endchar=*,evenchar=-,oddchar=-,sepchar=|"
0                   1                   2                   3  
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
*---------------------------------------------------------------*
| Field4| Field4|     Field8    |            Field16            |
*---------------------------------------------------------------*
|                            Field32                            |
*---------------------------------------------------------------*
|                                                               |
*                            Field64                            *
|                                                               |
*---------------------------------------------------------------*
```

```bash
protocol ~ protocol ip --bits 16
 0                   1          
 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Version|  IHL  |Type of Service|
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|          Total Length         |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|         Identification        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|Flags|     Fragment Offset     |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|  Time to Live |    Protocol   |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|        Header Checksum        |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                               |
+         Source Address        +
|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                               |
+      Destination Address      +
|                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|            Options            |
+               +-+-+-+-+-+-+-+-+
|               |    Padding    |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```