# DateTime::Grammar Raku package

## Introduction

This Raku package provides grammar (role) and interpreters for parsing datetime specifications. 

Most of the code is from [FS1]. The original code of the file 
["Parse.rakumod"](https://github.com/sergot/datetime-parse/blob/master/lib/DateTime/Parse.rakumod)
(from [FS1]) was separated into the files 
["Grammarish.rakumod"](./lib/DateTime/Grammarish.rakumod)
and
["Actions/Raku.rakumod"](./lib/DateTime/Actions/Raku.rakumod).

The code in
["Grammar.rakumod"](./lib/DateTime/Grammar.rakumod) 
provides the "top-level" functions:
- `datetime-parse`
- `datetime-subparse`
- `datetime-interpret`

**Remark:** The code `DateTime::Parse.new` can be replaced with `datetime-interpret`.
Compare the test files of this repository that have names starting with "01-" with the corresponding files in [FS1].


------

## Usage examples

Interpret a full blown datetime spec:

```perl6
use DateTime::Grammar;
my $rfc1123 = datetime-interpret('Sun, 06 Nov 1994 08:49:37 GMT');
$rfc1123.raku
```
```
# DateTime.new(1994,11,6,8,49,37)
```

Just the date:

```perl6
$rfc1123.Date;
```
```
# 1994-11-06
```

7th day of week:

```perl6
datetime-interpret('Sun', :rule<wkday>) + 1;
```
```
# 7
```

With the adverb `extended` we can control whether the datetime specs can be just dates. 
Here are examples:

```perl6
datetime-interpret('1/23/1089'):extended;
```
```
# 1089-01-23T00:00:00Z
```

```perl6
datetime-interpret('1/23/1089'):!extended;
```
```
# (Any)
```

------

## Using the role in "external" grammars

```perl6
my grammar DateTimeInterval 
    does DateTime::Grammarish {

    rule TOP { 'from' <from=.datetime-spec> 'to' <to=.datetime-spec> } 
};

DateTimeInterval.parse('from 2022-12-02 to Oct 4 2023')
```
```
# (Any)
```

------

## CLI

The package provides a Command Line Interface (CLI) script. Here is its usage message:

```shell
datetime-interpretation --help
```
```
# Usage:
#   datetime-interpretation <spec> [-t|--target=<Str>] -- Interpret datetime spec.
#   datetime-interpretation [<words> ...] [-t|--target=<Str>] -- Interpret datetime spec obtained by a sequence of strings.
#   datetime-interpretation [-t|--target=<Str>] -- Interpret datetime spec from pipeline input
#   
#     <spec>               Datetime specification.
#     -t|--target=<Str>    Interpretation target. [default: 'Raku']
#     [<words> ...]        Datetime specification.
```


------

## References

[FPS1] Filip Sergot,
[DateTime::Parse Raku package](https://github.com/sergot/datetime-parse),
(2017-2022),
[GitHub/sergot](https://github.com/sergot).