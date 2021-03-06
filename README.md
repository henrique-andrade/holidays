# holidays
This package returns country specific holidays in an easy manner. Currently the package comprises holidays for *Germany* only. However, some events -- such as Christmas days -- are also valid for many other countries.

Please ask questions and report bugs on the gretl mailing list if possible. Alternatively, open an issue ticket on my github repo.
Source code and test script(s) can be found here:
https://github.com/atecon/holidays

The package comprises four public functions. **First**, the user calls the *holiday()* function for obtaining a bundle of various binary series named according to a specific holiday. **Second**, the *get_list_of_holidays()* function is a convenience function, and appends the list of series from the previously returned bundle to the current data set. You may specify a string array for retrieving only specific holiday series. **Third**, the *print_names_of_holidays()* function simply prints all holidays in the compiled bundle. **Fourth**, the *get_names_of_holidays()* function returns a string array including the names of holidays compiled.

**Note**: The package only works if a time-series data set with periodicity 7 days is active. In case your active time-series data set has a 5 days periodicity, you may join the 7 days time-series to it.
Also, if you actually want to make use of holidays series for a panel data set, you need to join these to your panel data set by means of gretl's "join" or "append" command.

# Commented sample script

## Install the package from gretl's package server
```
pkg install holidays    # download from package server
```

## Retrieve list of holiday series for a country
```
clear
set verbose off

include holidays.gfn    # load functions into memory

# Define some daily time-series data set
nulldata 800
setobs 7 2018-01-01 --time-series

# Obtain holidays for Germany
bundle HD = holidays("de")
print HD
```

The resulting bundle *HD* includes the following items (just showing the first four items):
```
bundle HD, created by holidays:
  kindertag (series: length 800)
  fronleichnam (series: length 800)
  neujahr (series: length 800)
  pfingstmontag (series: length 800)
```

## Retrieve holidays as list of binary series
```
list L = get_list_of_holidays(HD, index)
summary L --simple			# descriptive statistics
print L -o --range=1:10
```
The first 10 obervations of the retrieved series (shown only for some of all series):
```
           christihimmelfahrt    frauentag ostersonntag pfingstmontag      neujahr

2018-01-01                  0            0            0             0            1
2018-01-02                  0            0            0             0            0
2018-01-03                  0            0            0             0            0
2018-01-04                  0            0            0             0            0
2018-01-05                  0            0            0             0            0
2018-01-06                  0            0            0             0            0
2018-01-07                  0            0            0             0            0
2018-01-08                  0            0            0             0            0
2018-01-09                  0            0            0             0            0
2018-01-10                  0            0            0             0            0

           fronleichnam    kindertag

2018-01-01            0            0
2018-01-02            0            0
2018-01-03            0            0
2018-01-04            0            0
2018-01-05            0            0
2018-01-06            0            0
2018-01-07            0            0
2018-01-08            0            0
2018-01-09            0            0
2018-01-10            0            0
```

## Return names of holidays as string array
```
strings holiday_names = get_names_of_holidays(HD)
eval holiday_names[1]
eval holiday_names[6]
```
The returned names of the first and sitxh list members are:
```
nikolaus
erster_mai
```
## Print names of holidays in bundle HD
```
print_names_of_holidays(HD)
```
The resulting list of holidays are (only showing the initial eight entries):
```
Names of holidays included in the bundle:
nikolaus
ostermontag
advent_4
aschermittwoch
valentinstag
erster_mai
silvester
allerheiligen
```

-----------------------------

# Public functions

Function:       holidays()
Arguments:      country_name (string, e.g. 'de')
Output:	        bundle

Return:
Bundle comprising a binary time-series for each holiday. The binary series takes the value of one if the specific holiday occurs at a certain date, otherwise zero.

-----------------------------------------------------------------------
Function:       get_list_of_holidays()
Arguments:      B (bundle returned by holidays() function)
                S (some arbitrary series of the active data set)
                hd (string array of holiday series to return, optional)
Output:         list

Return:
This convenience function appends all series of holidays in the bundle returned by the holidays() function to the active time-series data set. If one wants to retrieve only specific holidays, these can be defined by the optional string array 'hd'.

-----------------------------------------------------------------------
Function:       get_names_of_holidays()
Arguments:      B (bundle returned by holidays() function)
Output:         string array

Return:
This convenience function returns a string array holding all names of holidays compiled after having called holidays() function.

-----------------------------------------------------------------------
Function:       print_names_of_holidays()
Arguments:      B (bundle returned by holidays() function)
Output:         none

Return:
This convenience function print the unique names of holidays compiled after having called holidays() function.


# Adding holidays
Currently the package provides holidays only for Germany. If you would like to add holiday information for other countries that would be great! These are the steps to be done in case you want to add holidays for a new country:

1) Add a two-letter country flag to the 'countries' string array in
function countries_supported().
2) Add the corresponding case to the get_holidays() function.
3) Copy the days_with_fixed_date_de() function, name your new function days_with_fixed_date_<COUNTRYFLAG>() and define your set of holidays accordingly.
4) Copy the days_related_to_eastern_de function, name your new function days_related_to_eastern_<COUNTRYFLAG>() and define your set of holidays accordingly.

I recommend you to fork the source code from the github repo (https://github.com/atecon/holidays) and to send a pull request to me. However, if you are not familiar with the versioning tool git, you may contact me via email: <atecon@posteo.de>



## Changelog:
- v0.1, May 2020:
	+ initial release
