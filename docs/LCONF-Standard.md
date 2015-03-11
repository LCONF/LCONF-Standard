# The Official LCONF-Standard

**Unfinished Version 8.0.0 (20150813)**

MAJOR CHANGES:

*   The source of the documentation was rewritten in markdown format.
*   In the documentation `Key-Value-Mappings` are renamed to `Single-Blocks`.
*   Adds Reserved word: `___NOTSET`
*   Breaking changes:

    *   The *LCONF-Section* indentation level was previously exact 3 spaces per level. This new version of the
        LCONF-Standard allows that LCONF-Section specify there own number of spaces used per indentation level.
        For this reason the LCONF-Section start line format changed adding the indentation space number used for the
        section.
    *   *Key :: Value-Lists*: now comma and one space is used as separator for better readability
    *   Remove *Empty Key :: Value List* use: Empty Key-Value-List
    *   LCONF-SEPC 10 defines LCONF official supported types.

        *   Adds library requirements for *Value Transformation Types* and defines common string formats for such.

    *   *List-Of-Tuples* was replaced by a new **Tables** type.

<!-- TODO ============================================

*   Clarify `___NOTSET usage`
*   Allow Value separator to be only: `"::"` and strip space around it.
* maybe strip spaces in compact lists
================================================= -->


## Copyrights & Licenses

The **LCONF-Standard documentation** and associated documentation files
(the "DOCUMENTATION") is licensed under the following terms:

> Copyright (c) 2014, 2015: **peter1000**  <https://github.com/peter1000>. <br />
> All rights reserved.
>
> The DOCUMENTATION may be freely copied, published and distributed to
> others, provided that the above copyright notice and this Copyright
> License are included on all such copies or substantial portions of the
> DOCUMENTATION.
>
> However, the content of this DOCUMENTATION itself may not be modified
> in any way, including by removing the copyright notice, except as
> required to translate it into languages other than English or into a
> different format.
> In the event of discrepancies between a translated version and the
> official English version, the official English version shall govern.
>
> THIS DOCUMENTATION AND THE INFORMATION CONTAINED HEREIN IS PROVIDED
> "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING
> BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A
> PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS
> OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
> LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE,
> ARISING FROM, OUT OF OR IN CONNECTION WITH THE DOCUMENTATION OR THE
> USE OF THE INFORMATION HEREIN.



# 1. LCONF Introduction

**LCONF**: The light and simple readable data serialization format for dynamic configurations and data exchange.

It is a data serialization format designed *with special emphasis on being human-friendly* and works well with many
modern programming languages. This is a complete specification of the information needed to develop applications for
processing LCONF.

LCONF was specifically designed to be useful to people working with program configuration and configuration files as
well as for other common use cases such as: data exchange, interprocess messaging, log files, cross-language data
sharing.

It uses *Unicode printable characters*, containing the data itself and *Indentation* to provide structure in
combination with very few *Ascii characters* which provide structural information.
This excellent combination allows the data to show itself in a human-friendly, easily readable format.



## 1.1. Design Goals

The design goals for LCONF are, in no priority:

*   LCONF has minmal but strict structure.
*   LCONF is easily writeable and readable by humans.
*   LCONF allows optional comment lines which are not part of the actual data.
*   LCONF was designed from the start to expect a predefined known structure with implemented default data.



## 1.2. Relation To Other Data Serialization Formats

LCONF builds upon some concepts from [JSON](http://json.org/), [RSON](https://code.google.com/p/rson/),
[RSONLITE](https://pypi.python.org/pypi/rsonlite/0.1.0), [INI](http://en.wikipedia.org/wiki/INI_file), and
[YAML](http://yaml.org/) but has also a few destinct features.

In many situations `LCONF` is a suitable replacement for any of the others mentioned above.



## 1.3. Design Overview

### 1.3.1 Default Values

A major difference in design to many other data serialization formats is that LCONF was designed from the start to
expect a predefined known structure with implemented default data called `LCONF-Default-Template-Structure`.

Many programs which parse human written data input have to account that some parts of the input may be optional and
provide some means of checking for such and in some cases default values must be set.

*LCONF takes this as the base of its design* and **requires** that for any LCONF-Section a complete structure with
default values must be implemented. (`LCONF-Default-Template-Structure`)

Parsed data values of a LCONF-Section only overwrite the defined defaults or substitude the default value with a
predefined Replacement-Value.


### 1.3.2 Three Structures Which Hold Data

*   *Key :: Value Pair*: Associates a key with one data value.
*   *Table*: Associates key with a tabular-data values.
*   *List*: Associates a key with a list of data values.


### 1.3.3 Three Collection Structures

*   *LCONF-Root*: A collections of any of the five main structure types. (Key :: Value Pair, Table, List,
    Single-Block and Repeated-Block).
*   *Single-Block*: A collections of any of the five main structure types. (Key :: Value Pair, Table, List,
    Single-Block and Repeated-Block).
*   *Repeated-Block*: is similar to a Single-Block but additionally allow the configuration of multiple such blocks
    within a LCONF-Section.


### 1.3.4 Readability

To help to be easily writeable and readable by humans LCONF supports:

*   *Named-Sections*: which help to identify separate parts and allow for extended text/info/explanations before or
    after a LCONF-Section without the need of any Comment-TAGS. <br />
    One LCONF-Document can contain multiple LCONF-Sections.
*   *Empty Lines*: which are skipped but help to maintain readability.
*   *Comment Lines*: which are not part of the actual data.
*   *Indentation*: is used to visualize some of the structure of a LCONF-Section.


### 1.3.5 Order

LCONF was designed to be ordered based on the implemented `LCONF-Default-Template-Structure`. This means that emitting
of the same LCONF-Section will result always in an identical ordered representation.



# 2. LCONF Preview

This section provides a quick preview into LCONF-Sections. It is not expected that the first-time reader understand all
of the examples but should be seen as motivation for the remainder of the specification.

Some of the examples are based on [yaml's spec preview examples](http://www.yaml.org/spec/1.2/spec.html#Preview)

**Example 2.1. Key :: Value Pair: Mapping Keys to Values**

```text
name :: Tony Johnson
age :: 65
reg :: true
```

**Example 2.2. List: Mapping Keys to Value Sequences**

```text
- american
    Boston
    Detroit
    New York

# Compact form
- national :: New York, Chicago, Atlanta
```

**Example 2.3. Single-Blocks: Mapping Keys to Sequence of Mappings (Mapping of Mappings)**

```text
. player1
    name :: Mark McGwire
    hr :: 65
    avg  :: 0.278
. player2
    name :: Sammy Sosa
    hr :: 63
    avg :: 0.288
```

**Example 2.4. Tables: Mapping Keys to tabular-data (Sequence of Sequences)**

```text
| players
    | name         |hr  | avg   |
    | Mark McGwire | 65 | 0.278 |
    | Sammy Sosa   | 63 | 0.288 |
```

**Example 2.5. Two named LCON-Sections in one Document (Stream)**

```text
___SECTION :: 4 :: Ranking of 1998 home runs
- players
    Mark McGwire
    Sammy Sosa
    Ken Griffey
___END

___SECTION :: 4 :: Team ranking
- Ranking
    Chicago Cubs
    St Louis Cardinals
___END
```

**Example 2.6.1 Full Length Example 1**

```text
___SECTION :: 4 :: A short example
# Comment-Line
| People_Table
    |  Tim| 178| 86|
    | John| 166| 67|

* Persons_BLK
    person1
        first :: Tim
        last :: Doe
        age :: 39
        registered :: true
        salary :: 70000
        sex :: M
        - interests :: Reading, Mountain Biking, Hacking
        - sports
            tennis
            football
            soccer
        . favorites
            food :: Spaghetti
            sport :: Soccer
            color :: Blue
    person2
        first :: John
        last :: Doe
        age :: ___NOTSET
        registered :: ___NOTSET
        salary :: 45000
        sex :: M
        - sports
        . favorites
            food ::
            sport :: None
            color :: Orange
___END
```



# 3. LCONF Types

## 3.1. LCONF's Basic Types

### 3.1.1 Key :: Value Pair

Associates a key with one data value. <br />
**A valid value** is a sequence of **zero or more** Unicode characters till the end of the line.

`"Key-Name :: Value"`

(`one space, double colons, one space`) is used as a *Key-Value-Separator*.

**Exception:** for *Empty Values* the last space is skipped so that there is no trailing space.

```text
___SECTION :: 4 :: Example
Color :: Blue
FONT :: Liberation Mono
Sentence :: This is a long example value which can contain ...
# Comment below is an Empty Value with no trailing space.
MyEmptyKeyValuePair ::
___END
```

*Empty Key :: Value Pais values* are returned as empty strings or if defined per Replacement-Values.
Any Replacement-Value must be implemented in the `LCONF-Default-Template-Structure`.


### 3.1.2 Table

Associates a key with tabular-data values.
In most languages, this is realized as an multi-dimensional array, multi-dimensional grid, matrix, list of
lists, tables. <br />
**A valid value** is a sequence of **zero or more** Unicode characters.

`"| "`

(`pipe sign (vertical bar), one space`) is used as *Table-Identifier*.

Each LCONF-Table-Row starts on a separate line and uses one additional indented.
Values (Columns) of Table-Rows are embraced and separated by `"|"` `pipe sign (vertical bar)`.

Leading or ending whitespace of pipe separated data values are stripped.

**Exception:** for *Empty Tables* there is no new value row line.


TIP: to add Column Names use a comment line.

Restriction: each Table-Row must have the same number of column items as defined in the
`LCONF-Default-Template-Structure`.

```text
| Colors RGB
    #   Color Name| Red| Green| Blue|
    |  forestgreen|  34|   139|   34|
    |        brick| 156|   102|   31|
# Comment: Alignment is not important
| Colors RGB2
    |forestgreen|34|139|34|
    |brick|156|102|31|
# Comment: below is an empty Table
| MyEmptyTable
```

*Empty Table values* (missing values) are returned as empty strings or if defined per Replacement-Values.
Any Replacement-Value must be implemented in the `LCONF-Default-Template-Structure`.

```text
| ExcelTable
    # COMMENT: the 2. item is empty (missing)
    |value1|       | value3|
    |value1| value2| value3|
    # COMMENT: all items are empty (missing)
    |      |       |       |
    # COMMENT: the number of spaces are not important
    ||||
```


### 3.1.3 List

A list of values associates a key with an ordered list of data values. In most languages, this is realized as
an array, vector, list, or sequence. <br />
**A valid value** is a sequence of **one or more** Unicode characters.

LCONF supports two different notations for list types.

`"- "`

(`hyphen-minus ASCII character 45, one space`) is used as *List-Identifier* for both.

#### Key-Value-List Notation

*Multi-line* list: Uses a `List-Identifier` with a List-Name (key) and the values are separated by new lines and one
additional indented.

**Exception:** for *Empty Key-Value-Lists* there is no new value line.

```text
- names
    Tom
    Mary
    Peter
    Adrian
    Burton
    Calvin
    Daniel
# Comment below is an empty Empty Key-Value-Lists.
- MyEmptyList
```


#### Key :: Value-List Notation

*Single-line* list: Uses a `List-Identifier` with a List-Name (key) followed by a `Key-Value-Separator` and the
values are separated by `", "` (`comma, one space`). <br />
The values may not contain any leading nor ending whitespace.

Basically the same as `Key-Value-List` just uses a different notation for a very compact form.

**Exception:** There is no *Empty Key :: Value-List* use the *Key-Value-List* notation.

```text
- names :: Tom, Mary, Peter, Adrian, Burton, Calvin, Daniel
```



## 3.2 LCONF's Block Types

A collection of name(key)/value pairs. In various languages, this is realized as an object, struct.


### 3.2.1 Single-Block

A single collection of any of *3.1 LCONF's Basic Types* or *3.2 LCONF's Block Types*.

`". "`

(`dot, one space`)  is used as `Single-Block-Identifier` and *Block-Items* use **one** additional indentation level.

An *Empty Single-Block-Identifier* line is permitted which will use all default values as implemented by a
`LCONF-Template-Default-Structure`. It is basically the same as if one does not define it at all.

In some cases this might be useful: e.g. if one wants previous comment lines.

```text
___SECTION :: 4 :: SectionName
. Single-Block Identifier name
    single_block_item1_key :: single_block_item1_value
    - single_block_item2_key list
        my List-Item 1
        my List-Item 2
    # Comment: Blocks can also have other (nested) Blocks
    . inner_single_block_item3_key
        inner_single_block_item1_key :: inner_single_block_item1_value
# Comment: below a permitted empty `Single-Block-Identifier` which will use all default values
. Single-Block 2
___END
```


### 3.2.2 Repeated-Block

These are similar to the Single-Blocks but additionally allow the configuration of multiple such blocks within a
LCONF-Section.

A template collection of any of *3.1 LCONF's Basic Types* or *3.2 LCONF's Block Types*.
Repeated-Blocks are similar to Single-Blocks but additionally allow the configuration of multiple (repeated) such
blocks (each one is named) within a LCONF-Section.

`"* "`

(`asterisk, one space`) is used as `Repeated-Block-Identifier and unique `Block-Names` which uses **one** additional
indentation level define new `Blocks`` and are written on separate lines. Block-Name items use **another** additional
indentation level.

**NOTE**: to get the `Default-Values` for a whole Block: only define the Block-Identifier and  **Empty Block-Name*.
If a *Block-Name* is defined without any *Block-Items* at all it is still valid using all defaults for such Block from
the `LCONF-Template-Default-Structure`.

An *Empty Repeated-Block-Identifier* line is permitted but without any `Block-Name` it does nothing.
It is basically the same as if one does not define it at all.

In some cases this might be useful: e.g. if one wants previous comment lines.

```text
* Color_Repeated_BLK_Identifier
    # Comment: First Block-Name
    Blk-Name1 Sky Blue Theme
        blk_item_red :: 86
        blk_item_green :: 201
        blk_item_blue :: 241
        - blk_item_list :: Darker Blue, Dark Blue, Bright Blue, Brighter Blue
    # Comment: Second Block-Name
    Blk-Name2 Red Theme
        blk_item_red :: 212
        blk_item_green :: 24
        blk_item_blue :: 86
        - blk_item_list :: Darker Red, Dark Red, Bright Red, Brighter Red
    # Comment: Third Block-Name: is an empty Block which uses all default values
    Red Theme
# Comment: below a permitted empty `Repeated-Block-Identifier` which will do nothing.
* Repeated_BLK_Identifier 2
___END
```

Any number of *Block-Names* can be user defined: but a `LCONF-Template-Default-Structure` can also limit this.
LCONF libraries (parser/emitter) **must** implement support to easily support set/validate
`NUMBER_MIN_REQUIRED_BLOCKS`, `NUMBER_MAX_ALLOWED_BLOCKS`.


## 3.3 Root

A *LCONF-Root* is a collections of any of the five main structure types. (Key :: Value Pair, Table, List, Single-Block
and Repeated-Block).

All LCONF-Sections have at least the *LCONF-Root* type which is the level of LCONF-Items without any indentation.



## 3.4 LCONF's Value types

LCONF libraries (parser/emitter) **must** implement support to easily support the representation/conversion of the
following types:

*   String
*   Integer
*   Float
*   Number: Integer or Float
*   Boolean
*   Date
*   Time
*   DateTime
*   Range By Number Of Elements
*   Range By End Value
*   Value Not Set

**VERY IMPORTANT NOTE**

If a value is transformed to one of LCONF's supported types or stays as a string value **depends only** on the
implemented `LCONF-Template-Default-Structure`. There is **no distinction at all** in a LCONF-Section.

After the LCONF-Section below is parsed the value for `Age` could be one of these 3 types depending on the
`LCONF-Template-Default-Structure`:

*   LCONF String type
*   LCONF Number type
*   LCONF Integer

```text
___SECTION :: 4 :: Example
# Comment below is possible integer value
Age :: 36
___END
```


### 3.4.1 String

A value string of a LCONF *String* is a a sequence of zero or more Unicode characters.

LCONF-Section values (*String*) are never quoted and never contain any escaped characters.


### 3.4.2 Integer

A value string of a LCONF *Integer* must contain only digits which may be prefixed with a plus or minus sign.

*   64 bit (signed long) range expected (-9223372036854775808 to 9223372036854775807).

```text
Age :: 89

Price :: +18950

Saldo :: -800000
```


### 3.4.3 Float

A value string of a LCONF *Float* supports following notations.


#### Fractional Notation

An integer part (which may be prefixed with a plus or minus sign) followed by a fractional part. A fractional part is
a `"."` (decimal point - dot) followed by one or more digits.

```text
item1 :: +1.0

item2 :: -0.01

item3 :: 3.1415
```


#### Exponent Notation

An integer part (which may be prefixed with a plus or minus sign) followed by an exponent part. An exponent part is
an `"E"` or `"e"` (upper or lower case E) followed by an integer part (which may be prefixed with a plus or minus
sign).

```text
item1 :: 5e+22

item2 :: -1e6

item3 :: -2E-2
```


#### Fractional And Exponent Mixed Notation

An integer part (which may be prefixed with a plus or minus sign) followed by a fractional part followed by an exponent part. A fractional part is a `"."` (decimal point - dot) followed by one or more digits. An exponent part is an `"E"`
or `"e"` (upper or lower case E) followed by an integer part (which may be prefixed with a plus or minus sign).

```text
item1 :: 6.196e63

item2 :: -1.54e-003

item3 :: -1.54e+003

item4 :: 2.5e-4
```


#### Fraction P/Q Of Two Integers Notation

An integer part (which may be prefixed with a plus or minus sign) followed by a `"/"` (slash) followed by one or more
digits.

```text
item1 :: +3/4

item2 :: -93/16

item3 :: '1/8
```


### 3.4.4 Numbers

A value string of a LCONF *Number* can be either a **LCONF Integer** or **LCONF Float**.


### 3.4.5 Booleans

A value string of LCONF *Boolean* is a character sequence of `true` or `false`.

```text
item1 :: true

item2 :: false
```


### 3.4.6 Date

*   [YYYY] indicates a four-digit year, 0000 through 9999.
*   [MM] indicates a two-digit month of that year, 01 through 12.
*   [DD] indicates a two-digit day of that month, 01 through 31.

A value string of a LCONF *Date* is based on the ISO 8601 standard and supports following notations.


#### Month Notation

`YYYY-MM`: *four-digit year* followed by a `"-"` (hyphen-minus ASCII character 45) followed by *two-digit month* of
that year.

```text
BirthMonth :: 1932-08
```


#### Date Notation

`YYYY-MM-DD`: *four-digit year* followed by a `"-"` (hyphen-minus ASCII character 45) followed by *two-digit month* of
that year followed by a `"-"` (hyphen-minus ASCII character 45) followed by *two-digit day* of that month.

```text
BirthDay :: 1932-08-31
```


### 3.4.7 Time

*   [hh] refers to a zero-padded hour between 00 and 24 (where 24 is only used to denote midnight at the end of a
    calendar day).
*   [mm] refers to a zero-padded minute between 00 and 59.
*   [ss] refers to a zero-padded second between 00 and 60 (where 60 is only used to denote an added leap second).
*   [s] refers to one or more digits representing a decimal fraction of a second.

A value string of a LCONF *Time* is based on the ISO 8601 standard and supports following notations.


#### Minute Notation

`hh:mm`: *zero-padded hour* followed by a `":"` (colon) followed by *zero-padded minute*.

```text
StartMinute :: 12:30
```


#### Second Notation

`hh:mm:ss`: *zero-padded hour* followed by a `":"` (colon) followed by *zero-padded minute* followed by a `":"` (colon)
followed by *zero-padded second*.

```text
StartSecond :: 12:30:42
```


#### Second Fraction Notation

`hh:mm:ss.s`: *zero-padded hour* followed by a `":"` (colon) followed by *zero-padded minute* followed by a
`":"` (colon) followed by *zero-padded second*  followed by a `"."` (decimal point - dot) followed by one or more
digits representing a decimal fraction of a second.

```text
StartFractionSecond :: 12:30:59.001

StartFractionSecond :: 18:53:16.1

StartFractionSecond :: 04:02:00.000156
```


### 3.4.8 DateTime

LCONF *DateTime* is a combination of a **LCONF Date** and **LCONF Time*** and has the letter `"T"` (capital T) as a
delimiter between the *Date* and *Time* part.

A value string of a LCONF *DateTime* is based on the ISO 8601 standard and supports following notations.


#### Minute Notation

`YYYY-MM-DDThh:mm`

```text
StartMinute :: 2013-07-01T12:30
```


#### Second Notation

`YYYY-MM-DDThh:mm:ss`

```text
StartSecond :: 2013-07-01T12:30:59
```

Notation
#### Second Fraction Notation

`YYYY-MM-DDThh:mm:ss.s`

```text
StartFractionSecond :: 2013-07-01T12:30:59.001

StartFractionSecond :: 2013-07-01T18:53:16.1

StartFractionSecond :: 2013-07-01T04:02:00.000156
```


### 3.4.9 Range By Number Of Elements

A value string of LCONF *Range By Number Of Elements* defines an arithmetic sequence where the first element is the
*Start-Number*.

`Start-Number|Step-Number|*Integer Of Number Of Elements`

*Start-Number* (Integer or Float) followed by a `"|"` `pipe sign (vertical bar)` followed by *Step-Number* (Integer or
Float) followed by `"|"` `pipe sign (vertical bar)` followed by a `"*"` (asterisk) followed by *Integer Of Number Of
Elements* must be greater than zero.

Restriction: The *Step-Number* may not be equal to 0 (zero).

```text
repeating :: -10|1|*21

repeating :: 512.4|0.125|*8
```

*   `-10|1|*21` represents a sequence of:

    `-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10`

*   `512.4|0.125|*8` represents a sequence of:

    `512.4, 512.275, 512.15, 512.025, 511.9, 511.775, 511.65, 511.525`


### 3.4.10 Range By End Value

A value string of LCONF *Range By End Value* defines an arithmetic sequence where the first element is the
*Start-Number*.

`Start-Number|Step-Number|End-Number` or `Start-Number|Step-Number|End-Number|force`

*Start-Number* (Integer or Float) followed by a `"|"` `pipe sign (vertical bar)` followed by *Step-Number* (Integer or
Float) followed by `"|"` `pipe sign (vertical bar)` followed by *End-Number* (Integer or Float).

To force always the inclusion of the *End-Number* it can be followed by `"|"` `pipe sign (vertical bar)` and the
character sequence `force`.

Restriction: The *Step-Number* may not be equal to 0 (zero).


#### Positive Step-Number value

If the Step-Number value is greater than zero then the *Start-Number* must be less or equal to the *End-Number*. The
last element is the largest: `Start-Number + i * Step-Number less or equal to End-Number`.

```text
repeating :: -10|1|5

repeating :: -10|1|5|force

repeating :: 100.8|1.27|106

repeating :: 100.8|1.27|106|force
```

*   `-10|1|5` represents a sequence of:

    `-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5`

*   `-10|1|5|force` represents a sequence of:

    `-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0, 1, 2, 3, 4, 5`


*   `100.8|1.27|106` represents a sequence of:

    `100.8, 102.07, 103.34, 104.61, 105.88`


*   `100.8|1.27|106|force` represents a sequence of:

    `100.8, 102.07, 103.34, 104.61, 105.88, 106`


#### Negative Step-Number value

If the Step-Number value is less than zero then the *Start-Number* must be greater or equal to the *End-Number*. The
last element is the smallest: `Start-Number + i * Step-Number greater or equal to End-Number`.

```text
repeating :: 10|1|-5

repeating :: 10|1|-5|force

repeating :: 100.8|-1.27|92.1

repeating :: 100.8|-1.27|92.1|force
```

*   `10|1|-5` represents a sequence of:

    `10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0, -1, -2, -3, -4, -5`

*   `10|1|-5|force` represents a sequence of:

    `10, 9, 8, 7, 6, 5, 4, 3, 2, 1, 0, -1, -2, -3, -4, -5`

*   `100.8|-1.27|92.1` represents a sequence of:

    `100.8, 99.53, 98.26, 96.99, 95.72, 94.45, 93.18`

*   `100.8|-1.27|92.1|force` represents a sequence of:

    `100.8, 99.53, 98.26, 96.99, 95.72, 94.45, 93.18, 92.1`


### 3.4.11 Value Not Set

`"___NOTSET"`

(`three underlines, capital NOTSET`)

A value string of LCONF *Value Not Set* is a character sequence of `___NOTSET`.
This is also a LCONF Reserved word.

A `___NOTSET` value is used to indicate the lack of a value.
**Note**: a `___NOTSET` is different from an empty string value.



# 4. LCONF General Specifications

## 4.1 Key-Value-Separator

`" :: "`

(`one space, double colons, one space`) is used as a *Key-Value-Separator*.



## 4.2 Trailing Space

LCONF does not allow any *Trailing Space* within a LCONF-Section.



## 4.3 Indentation

LCONF allows only spaces to be used as indentation. The number of spaces used per indentation level is set within
the LCONF-Section-Start-Line.

`"___SECTION :: Number-Of-Spaces-Used-Per-Indentation-Level :: Section-Name"`

`three underlines, capital SECTION` followed by a *Key-Value-Separator* folloed by the number of spaces used per indentation level  followed by a *Key-Value-Separator* followed by the Section-Name given to this section is used as
the *LCONF-Section-Start-Line*.


The example below would specify that 4 spaces are used per intentation level within the LCONF-Section named *Example*

```text
___SECTION :: 4 :: Example
___END
```



## 4.4 Named-Section

LCONF-Documents can contain multiple named LCONF-Sections. A LCONF-Section has a clear defined Start-TAG and End-TAG.
Everything outside of these is considered *additional text*.

**WARNING:** LCONF-Section Start/End TAGS are forbidden in any form except for the defined purpose.


### 4.4.1 Section Start TAG

`"___SECTION"`

(`three underlines, capital SECTION`) is used as LCONF's *Section-Start-TAG*.

The *LCONF-Section-Start-TAG* must always be without any indentation.

```text
___SECTION :: 4 :: the name can also contain spaces or unicode
```


### 4.4.2 Section End TAG

`"___END"`

(`three underlines, capital END`) is used as LCONF's *Section-End-TAG*

The *LCONF-Section-End-TAG* must always be without any indentation.



## 4.5 Simple Valid LCONF-Section

The most basic valid LCONF-Section is:

```text
___SECTION :: 4 :: Example
___END
```



## 4.6 Comment-Lines

`"#"`

(`one number sign`) (hash symbol) is used as *Comment-Line-Identifier*.

If the first none white character in a LCONF-Section line is **#** the line is considered a Comment-Line.

Comment-Lines **must** have the indentation level of the following line (disregarding empty lines).
Comment-Lines within a LCONF-Section are always skipped when the `LCONF-Section` is parsed.

```text
___SECTION :: 4 :: Example
# Comment-Line more info
- Names
    Tim
    Sandra
    # Comment-Line must have the indentation level of the following line
    Max
    Frank
___END
```

**NOTE:** Default *Comment-Lines and Default-Empty-Lines* which are implemented in the
`LCONF-Default-Template-Structure` can be optionally emitted.



## 4.7 Replacement-Value

As mentioned under the specific types LCONF *Key :: Value Pair* and *Table* can have value items which are empty.
These are returned as empty strings or if defined per Replacement-Values.
Any Replacement-Value must be implemented in the `LCONF-Default-Template-Structure`.

LCONF libraries (parser/emitter) **must** implement support to easily set Replacement-Values in a
`LCONF-Default-Template-Structure`.

For LCONF *Key :: Value Pairs* there must be the option to set Replacement-Values per each Key :: Value Pair.

For LCONF *Tables* there must be the option to set Replacement-Values per Column.



# 5. LCONF Summary of Restrictions

#### Restriction: Section Start/End Tag

Character combinations `___SECTION`, `___END` are forbidden in any form except for the defined purpose.
Each LCONF-Section-Start-TAG must be closed by an corresponding LCONF-Section-End-TAG.

LCONF does not allow nested Sections.


#### Restriction: Value Not Set

Character combination `___NOTSET` is forbidden in a LCONF-Section except for the defined purpose if a value is not set.

A `___NOTSET` value is used to indicate the lack of a value.
**Note**: that a `___NOTSET` is different from an empty string.


#### Restriction: Trailing Space

LCONF does not allow any Trailing Space within LCONF-Sections.


#### Restriction: First None White Character Of A Line

Some first none white character of a line are **reserved** as special purpose *Identifiers*. Any of them are permitted
in the middle or end of a line.

*   `| pipe sign (vertical bar)` is reserved only for `Table-Identifier` and Table-Rows.
*   `- hyphen-minus ASCII character 45` is reserved only for `List-Identifier`
*   `. dot` is reserved only for `Single-Block-Identifier`
*   `* asterisk` is reserved only for `Repeated-Block-Identifier`
*   `# number sign (hash symbol)` is reserved only for `Comment-Line-Identifier`


#### Restriction: Table Value Rows

*Tables* must contain the same number of row values as column-names specified in the
`LCONF-Default-Template-Structure`.


#### Restriction: Comment Lines Indentation

**#** Comment lines within a `LCONF-Section` are required to have the indentation of the next none empty line.


#### Restriction: Unique Names

LCONF requires that some of the Key-Names are unique.

*   all `Root Keys` (keys without any indentation) must be unique.

*   withing a `Single-Block` all direct keys (keys with one additional indentation level) must be unique.

*   within a `Repeated-Block` all direct keys (keys with one additional indentation level - *Block-Names*) must be
    unique.

*   within each `Named-Block` of a *Repeated-Block* all direct keys (keys with one additional indentation level) must
    be unique.



<!-- # 6. LCONF-Default-Template-Structure

LCONF **requires** a parser/emitter to implement for any LCONF-Section a complete structure with default values. Such
`LCONF-Default-Template-Structure` implementation may differ from programming language and parser/emitter but there are
some common requirements outlined below.



## 6.1 LCONF-Section Key Order

LCONF-Structure is ordered so that emitting of the same LCONF-Document will result always in an identical
representation. The order is always based on the implemented `LCONF-Default-Template-Structure` and not on the
`LCONF-Section text`.

**Exception:**

*   The order of *Block-Names* of Repeated-Blocks will always be as as in the `LCONF-Section text` because these are not
    previously known.

*   The order of *Lists items* will always be as in the `LCONF-Section text` because these are not previously known.



## 6.2 Default-Comment/Empty Lines

`LCONF-Default-Template-Structure` can implement *Default-Comment/Empty Lines* which can be optionally emitted.

**Restrictions: Default-Comment/Empty Lines**

*   before `Block-Names` (dummy blk) there may be no `Default-Comment/Empty Lines` within the code of the
  `LCONF-Default-Template-Structure`.

*   within multi-line `Lists` *Key-Value-Lists* or *List-Of-Tuples* there may be no `Default-Comment/Empty Lines`
    between default List-Items within the code of the `LCONF-Default-Template-Structure`.



## 6.3 Default Values

LCONF is based on the idea of a predefined `LCONF-Default-Template-Structure` which must fully implement any
LCONF-Section.

*   This gives it order, default values and one knows what to expect.
*   This helps to emit/dump in proper order based on the implemented structure.

    *   inclusive any `Default-Comment/Empty Lines`
    *   Any LCONF library **must** implement an option to emit/dump any `Repeated-Block` with an commented
        'Dummy-Block' with its default values.

*   Only a few things are not pre-known:

    *   Any LCONF-Section set values.
    *   The number of items in lists.
    *   The number of Block-Names in `Repeated-Blocks`.

      *   For special purposes `LCONF-Template-Default-Structure` Repeated-Blocks implementation can optional predefine
        some limitations: NUMBER_MIN_REQUIRED_BLOCKS, NUMBER_MAX_ALLOWED_BLOCKS

*   Because all structures must be previously implemented within the code any library which implements *LCONF: The
    light and simple readable data serialization format* should give some thoughts as how do write such
    `LCONF-Template-Default-Structure` as easily as possible.

*   Parsing a LCONF-Section (text/file) will just overwrite any default values.

    So the simplest LCONF-Section is only a START/END TAG:
      which will be parsed to all implemented defaults as nothing gets overwritten.

      NOTE: There won't be any Repeated-Blocks because there are no default Block-Names set.



## 6.4 Value Transformation

By design LCONF only supports *string* types for data values.

However `LCONF-Libraries` **must** implement an easy way for value transformation using customary hook functions or
some other ways to achieve such depending on the library language. See document section:
*3.1 LCONF's Data Types/Value Transformation Types*



## 6.5 Optional Replacement-Values for empty string values.

 `LCONF-Libraries` **must** implement a way to specify optional Replacement-Values for -->
