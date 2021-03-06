#Sort By

##Definition

The **sort** verb sorts data in a [Data package](../../package.html) by one or several criteria. The result is a [Data package](../../package). Standard alphabetical, chronological or numerical orders are used. 


##Syntax
<script type="text/javascript">
Diagram(
OneOrMore(NonTerminal('DATA PACKAGE')),  

Terminal('~>'),

    Terminal('sort by'),

Choice(0,
  Terminal('time'),
  NonTerminal('CHOPSTICK')
),
Choice(0,
  Terminal('asc'),
  Terminal('desc')
),
     

Terminal('~>'),
OneOrMore(NonTerminal('DATA PACKAGE'))
).addTo();
</script>

```language-tractor
~> sort by OPTIONS ORDER~>
```

Options:
- time
```
language-tractor
~> sort by time ~>
```
- [Context type chopstick](../../chopstick.html#context) : [Country:], [Brand:][Country:], etc
```
language-tractor
~> sort by [Country:] ~>
```
- [Signal type selector](../../chopstick.html#signal) : {Net:}, {Net:}{Tax:}
```
language-tractor
~> sort by {Net:}{Tax:} ~>
```

Order
- asc
- desc
```
language-tractor
~> sort by time asc ~>
```

##Examples

###Example 1: Sort Points Chronologically 

Take the following data set:

```language-katsu
2013-09-06[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:883.78}
2013-09-03[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:866.19}
2013-09-04[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:873.5}
2013-09-10[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:892}
2013-09-05[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:879.88}
2013-09-12[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:897.9}
2013-09-13[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:895.68}
2013-09-09[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:889.75}
2013-09-04[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:873.5}
2013-09-11[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:896.97}
2013-09-16[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:897}
```
The following script orders the points from the oldest to the latest:
```language-tractor
~> sort by time ~>
```

and returns
```language-katsu
2013-09-03[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:866.19}
2013-09-04[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:873.5}
2013-09-05[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:879.88}
2013-09-06[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:883.78}
2013-09-09[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:889.75}
2013-09-10[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:892}
2013-09-11[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:896.97}
2013-09-12[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:897.9}
2013-09-13[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:895.68}
2013-09-16[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:897}
```

###Example 2: Sort Points From Latest To Oldest 

Take the following data set:

```language-katsu
2013-01-01[Brand:iPhone][Country:Germany]{Net:10}{Tax:2}{Gross:12}
2014-01-01[Brand:Samsung Galaxy][Country:Germany]{Net:10}{Tax:1}{Gross:11}
2013-01-02[Brand:iPhone][Country:UK]{Net:12}{Tax:2}{Gross:14}
2014-01-03[Brand:iPhone][Country:UK]{Net:12}{Tax:1}{Gross:13}
2014-01-03[Brand:Samsung Galaxy][Country:France]{Net:25}{Tax:3}{Gross:28}
2014-03-04[Brand:iPhone][Country:France][City:Paris]{Before Tax:10}{Tax:1}{After Tax:12}
```
The following script orders the points from the latest to oldest:

```language-tractor
~> sort by time desc ~>
```
returns
```language-katsu
2014-03-04[Brand:iPhone][Country:France][City:Paris]{Before Tax:10}{Tax:1}{After Tax:12}
2014-01-03[Brand:Samsung Galaxy][Country:France]{Net:25}{Tax:3}{Gross:28}
2014-01-03[Brand:iPhone][Country:UK]{Net:12}{Tax:1}{Gross:13}
2014-01-01[Brand:Samsung Galaxy][Country:Germany]{Net:10}{Tax:1}{Gross:11}
2013-01-02[Brand:iPhone][Country:UK]{Net:12}{Tax:2}{Gross:14}
2013-01-01[Brand:iPhone][Country:Germany]{Net:10}{Tax:2}{Gross:12}
```

###Example 3: Sort Points By Context Type

The **sort** verb accepts a list of context types on which the sort operation is performed successively. 

For instance, take the following data set:

```language-katsu
2013-01-01[Brand:iPhone][Country:Germany]{Net:10}{Tax:2}{Gross:12}
2014-01-01[Brand:Samsung Galaxy][Country:Germany]{Net:10}{Tax:1}{Gross:11}
2013-01-02[Brand:iPhone][Country:UK]{Net:12}{Tax:2}{Gross:14}
2014-01-03[Brand:iPhone][Country:UK]{Net:12}{Tax:1}{Gross:13}
2014-01-03[Brand:Samsung Galaxy][Country:France]{Net:25}{Tax:3}{Gross:28}
2014-03-04[Brand:iPhone][Country:France][City:Paris]{Before Tax:10}{Tax:1}{After Tax:12}
```
The following script orders the data by Brand and then by Country:

```language-tractor
~> sort by [Brand:][Country:] ~>
```
and returns:
```language-katsu
2014-03-04[Brand:iPhone][Country:France][City:Paris]{Before Tax:10}{Tax:1}{After Tax:12}
2013-01-01[Brand:iPhone][Country:Germany]{Net:10}{Tax:2}{Gross:12}
2013-01-02[Brand:iPhone][Country:UK]{Net:12}{Tax:2}{Gross:14}
2014-01-03[Brand:iPhone][Country:UK]{Net:12}{Tax:1}{Gross:13}
2014-01-03[Brand:Samsung Galaxy][Country:France]{Net:25}{Tax:3}{Gross:28}
2014-01-01[Brand:Samsung Galaxy][Country:Germany]{Net:10}{Tax:1}{Gross:11}
```

Alternatively,
```language-tractor
~> sort by [Country:]desc[Brand:] ~>
```
orders by Country (with reverse alphabetical order) and then by Brand:
```language-katsu
2013-01-02[Brand:iPhone][Country:UK]{Net:12}{Tax:2}{Gross:14}
2014-01-03[Brand:iPhone][Country:UK]{Net:12}{Tax:1}{Gross:13}
2013-01-01[Brand:iPhone][Country:Germany]{Net:10}{Tax:2}{Gross:12}
2014-01-01[Brand:Samsung Galaxy][Country:Germany]{Net:10}{Tax:1}{Gross:11}
2014-03-04[Brand:iPhone][Country:France][City:Paris]{Before Tax:10}{Tax:1}{After Tax:12}
2014-01-03[Brand:Samsung Galaxy][Country:France]{Net:25}{Tax:3}{Gross:28}
```

The time and values of the Katsu data are ignored here. To sort by more than one dimention, refer to the things to note section of this page.

###Example 4: Sort Points By Values

Take the following data set:

```language-katsu
2013-09-03[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:866.19}
2013-09-04[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:873.5}
2013-09-05[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:879.88}
2013-09-06[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:883.78}
2013-09-09[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:889.75}
2013-09-10[Company:Google][Source:YahooFinance][Symbol:GOOG]
2013-09-11[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:896.97}
2013-09-12[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:897.9}
2013-09-13[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:895.68}
2013-09-16[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:897}
```
The following script orders the points by values of the 'Net' signal:
```language-tractor
~> sort by {High:} ~>
```
and returns:
```language-katsu
2013-09-12[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:897.9}
2013-09-16[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:897}
2013-09-11[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:896.97}
2013-09-13[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:895.68}
2013-09-09[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:889.75}
2013-09-06[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:883.78}
2013-09-05[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:879.88}
2013-09-04[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:873.5}
2013-09-03[Company:Google][Source:YahooFinance][Symbol:GOOG]{High:866.19}
2013-09-10[Company:Google][Source:YahooFinance][Symbol:GOOG]
```
Data points without a High signal are placed at the end of the stack.

###Example 5: Sort Points By Two Different Values

Take the following data set:

```language-katsu
2013-01-01[Brand:iPhone][Country:Germany]{Net:10}{Tax:2}{Gross:12}
2014-01-01[Brand:Samsung Galaxy][Country:Germany]{Net:10}{Tax:1}{Gross:11}
2013-01-02[Brand:iPhone][Country:UK]{Net:12}{Tax:2}{Gross:14}
2014-01-03[Brand:iPhone][Country:UK]{Net:12}{Tax:1}{Gross:13}
2014-01-03[Brand:Samsung Galaxy][Country:France]{Net:25}{Tax:3}{Gross:28}
2014-03-04[Brand:iPhone][Country:France][City:Paris]{Before Tax:10}{Tax:1}{After Tax:12}
```
This script orders the points by values of the Net signal, followed by the Tax signal:

```language-tractor
~> sort by {Net:}{Tax:} ~>
```
Data points without a Net signal are placed at the end of the stack:
```language-katsu
2014-01-01[Brand:Samsung Galaxy][Country:Germany]{Net:10}{Tax:1}{Gross:11}
2013-01-01[Brand:iPhone][Country:Germany]{Net:10}{Tax:2}{Gross:12}
2014-01-03[Brand:iPhone][Country:UK]{Net:12}{Tax:1}{Gross:13}
2013-01-02[Brand:iPhone][Country:UK]{Net:12}{Tax:2}{Gross:14}
2014-01-03[Brand:Samsung Galaxy][Country:France]{Net:25}{Tax:3}{Gross:28}
2014-03-04[Brand:iPhone][Country:France][City:Paris]{Before Tax:10}{Tax:1}{After Tax:12}
```

###Things To Note With Sort By

The Sort by verb can be used consecutively to order datapoints by one dimenstion after the other. For example, this script orders by time in reverse, followed by values of the Net signal, followed by the country and reverse Brand:

```language-tractor
~> sort by time desc ~> sort by {Net:} ~> sort by [Country:][Brand:]desc
```
The the example dataset:
```language-katsu
2013-01-01[Brand:iPhone][Country:Germany]{Net:10}{Tax:2}{Gross:12}
2014-01-01[Brand:Samsung Galaxy][Country:Germany]{Net:10}{Tax:1}{Gross:11}
2013-01-02[Brand:iPhone][Country:UK]{Net:12}{Tax:2}{Gross:14}
2014-01-03[Brand:iPhone][Country:UK]{Net:12}{Tax:1}{Gross:13}
2014-01-03[Brand:Samsung Galaxy][Country:France]{Net:25}{Tax:3}{Gross:28}
2014-03-04[Brand:iPhone][Country:France][City:Paris]{Before Tax:10}{Tax:1}{After Tax:12}
```

the following would be returned:
```language-katsu
2014-03-04[Brand:iPhone][Country:France][City:Paris]{Before Tax:10}{Tax:1}{After Tax:12}
2014-01-01[Brand:Samsung Galaxy][Country:Germany]{Net:10}{Tax:1}{Gross:11}
2013-01-01[Brand:iPhone][Country:Germany]{Net:10}{Tax:2}{Gross:12}
2014-01-03[Brand:iPhone][Country:UK]{Net:12}{Tax:1}{Gross:13}
2014-01-03[Brand:Samsung Galaxy][Country:France]{Net:25}{Tax:3}{Gross:28}
2013-01-02[Brand:iPhone][Country:UK]{Net:12}{Tax:2}{Gross:14}
```
