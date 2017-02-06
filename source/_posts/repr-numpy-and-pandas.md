---
title: Numpy和Pandas教程
toc: true
date: 2017-02-05 09:00:00
tags:
- 转载
- 机器学习
- numpy
- pandas
- python
categories: 精华转载
---
# 前言
本文转载自[优达学城《机器学习工程师》](https://cn.udacity.com/course/machine-learning-engineer-nanodegree--nd009)

既然目前为止，你已学会了一些基本的统计学概念，现在让我们探讨一些 Python 库，它们允许您研究数据和处理大型数据集。

具体而言，在本阶段的课程中，我们将探讨 numpy，它允许您处理大量数值数据以及 panda 序列和数据框（它们允许你存储大型数据集和提取其中的信息）。我们将学习 numpy 和 panda.DataFrames，前者能够帮助你处理大量数值数据，而后者可以帮助你存储大型数据集以及从数据集中提取出来的信息。

Numpy 库文档： https://docs.scipy.org/doc/numpy-dev/user/quickstart.html

Pandas 库文档： http://pandas.pydata.org/pandas-docs/version/0.17.0/

<!--more-->

# Numpy
```
import numpy as np

'''
The following code is to help you play with Numpy, which is a library 
that provides functions that are especially useful when you have to
work with large arrays and matrices of numeric data, like doing 
matrix matrix multiplications. Also, Numpy is battle tested and 
optimized so that it runs fast, much faster than if you were working
with Python lists directly.
'''

'''
The array object class is the foundation of Numpy, and Numpy arrays are like
lists in Python, except that every thing inside an array must be of the
same type, like int or float.
'''
# Change False to True to see Numpy arrays in action
if False:
    array = np.array([1, 4, 5, 8], float)
    print array
    print ""
    array = np.array([[1, 2, 3], [4, 5, 6]], float)  # a 2D array/Matrix
    print array

'''
You can index, slice, and manipulate a Numpy array much like you would with a
a Python list.
'''
# Change False to True to see array indexing and slicing in action
if False:
    array = np.array([1, 4, 5, 8], float)
    print array
    print ""
    print array[1]
    print ""
    print array[:2]
    print ""
    array[1] = 5.0
    print array[1]

# Change False to True to see Matrix indexing and slicing in action
if False:
    two_D_array = np.array([[1, 2, 3], [4, 5, 6]], float)
    print two_D_array
    print ""
    print two_D_array[1][1]
    print ""
    print two_D_array[1, :]
    print ""
    print two_D_array[:, 2]

'''
Here are some arithmetic operations that you can do with Numpy arrays
'''
# Change False to True to see Array arithmetics in action
if False:
    array_1 = np.array([1, 2, 3], float)
    array_2 = np.array([5, 2, 6], float)
    print array_1 + array_2
    print ""
    print array_1 - array_2
    print ""
    print array_1 * array_2

# Change False to True to see Matrix arithmetics in action
if False:
    array_1 = np.array([[1, 2], [3, 4]], float)
    array_2 = np.array([[5, 6], [7, 8]], float)
    print array_1 + array_2
    print ""
    print array_1 - array_2
    print ""
    print array_1 * array_2

'''
In addition to the standard arthimetic operations, Numpy also has a range of
other mathematical operations that you can apply to Numpy arrays, such as
mean and dot product.

Both of these functions will be useful in later programming quizzes.
'''
if False:
    array_1 = np.array([1, 2, 3], float)
    array_2 = np.array([[6], [7], [8]], float)
    print np.mean(array_1)
    print np.mean(array_2)
    print ""
    print np.dot(array_1, array_2)

```

# Pandas

```
import pandas as pd

'''
The following code is to help you play with the concept of Series in Pandas.

You can think of Series as an one-dimensional object that is similar to
an array, list, or column in a database. By default, it will assign an
index label to each item in the Series ranging from 0 to N, where N is
the number of items in the Series minus one.

Please feel free to play around with the concept of Series and see what it does

*This playground is inspired by Greg Reda's post on Intro to Pandas Data Structures:
http://www.gregreda.com/2013/10/26/intro-to-pandas-data-structures/
'''
# Change False to True to create a Series object
if False:
    series = pd.Series(['Dave', 'Cheng-Han', 'Udacity', 42, -1789710578])
    print series

'''
You can also manually assign indices to the items in the Series when
creating the series
'''

# Change False to True to see custom index in action
if False:
    series = pd.Series(['Dave', 'Cheng-Han', 359, 9001],
                       index=['Instructor', 'Curriculum Manager',
                              'Course Number', 'Power Level'])
    print series

'''
You can use index to select specific items from the Series
'''
# Change False to True to see Series indexing in action
if False:
    series = pd.Series(['Dave', 'Cheng-Han', 359, 9001],
                       index=['Instructor', 'Curriculum Manager',
                              'Course Number', 'Power Level'])
    print series['Instructor']
    print ""
    print series[['Instructor', 'Curriculum Manager', 'Course Number']]

'''
You can also use boolean operators to select specific items from the Series
'''
# Change False to True to see boolean indexing in action
if False:
    cuteness = pd.Series([1, 2, 3, 4, 5], index=['Cockroach', 'Fish', 'Mini Pig',
                                                 'Puppy', 'Kitten'])
    print cuteness > 3
    print ""
    print cuteness[cuteness > 3]

```

# Pandas 数据框

```
import numpy as np
import pandas as pd

'''
The following code is to help you play with the concept of Dataframe in Pandas.

You can think of a Dataframe as something with rows and columns. It is
similar to a spreadsheet, a database table, or R's data.frame object.

*This playground is inspired by Greg Reda's post on Intro to Pandas Data Structures:
http://www.gregreda.com/2013/10/26/intro-to-pandas-data-structures/
'''

'''
To create a dataframe, you can pass a dictionary of lists to the Dataframe
constructor:
1) The key of the dictionary will be the column name
2) The associating list will be the values within that column.
'''
# Change False to True to see Dataframes in action
if False:
    data = {'year': [2010, 2011, 2012, 2011, 2012, 2010, 2011, 2012],
            'team': ['Bears', 'Bears', 'Bears', 'Packers', 'Packers', 'Lions',
                     'Lions', 'Lions'],
            'wins': [11, 8, 10, 15, 11, 6, 10, 4],
            'losses': [5, 8, 6, 1, 5, 10, 6, 12]}
    football = pd.DataFrame(data)
    print football

'''
Pandas also has various functions that will help you understand some basic
information about your data frame. Some of these functions are:
1) dtypes: to get the datatype for each column
2) describe: useful for seeing basic statistics of the dataframe's numerical
   columns
3) head: displays the first five rows of the dataset
4) tail: displays the last five rows of the dataset
'''
# Change False to True to see these functions in action
if False:
    data = {'year': [2010, 2011, 2012, 2011, 2012, 2010, 2011, 2012],
            'team': ['Bears', 'Bears', 'Bears', 'Packers', 'Packers', 'Lions',
                     'Lions', 'Lions'],
            'wins': [11, 8, 10, 15, 11, 6, 10, 4],
            'losses': [5, 8, 6, 1, 5, 10, 6, 12]}
    football = pd.DataFrame(data)
    print football.dtypes
    print ""
    print football.describe()
    print ""
    print football.head()
    print ""
    print football.tail()

```


```
from pandas import DataFrame, Series

#################
# Syntax Reminder:
#
# The following code would create a two-column pandas DataFrame
# named df with columns labeled 'name' and 'age':
#
# people = ['Sarah', 'Mike', 'Chrisna']
# ages  =  [28, 32, 25]
# df = DataFrame({'name' : Series(people),
#                 'age'  : Series(ages)})

def create_dataframe():
    '''
    Create a pandas dataframe called 'olympic_medal_counts_df' containing
    the data from the table of 2014 Sochi winter olympics medal counts.  

    The columns for this dataframe should be called 
    'country_name', 'gold', 'silver', and 'bronze'.  

    There is no need to  specify row indexes for this dataframe 
    (in this case, the rows will automatically be assigned numbered indexes).
    
    You do not need to call the function in your code when running it in the
    browser - the grader will do that automatically when you submit or test it.
    '''

    countries = ['Russian Fed.', 'Norway', 'Canada', 'United States',
                 'Netherlands', 'Germany', 'Switzerland', 'Belarus',
                 'Austria', 'France', 'Poland', 'China', 'Korea', 
                 'Sweden', 'Czech Republic', 'Slovenia', 'Japan',
                 'Finland', 'Great Britain', 'Ukraine', 'Slovakia',
                 'Italy', 'Latvia', 'Australia', 'Croatia', 'Kazakhstan']

    gold = [13, 11, 10, 9, 8, 8, 6, 5, 4, 4, 4, 3, 3, 2, 2, 2, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0]
    silver = [11, 5, 10, 7, 7, 6, 3, 0, 8, 4, 1, 4, 3, 7, 4, 2, 4, 3, 1, 0, 0, 2, 2, 2, 1, 0]
    bronze = [9, 10, 5, 12, 9, 5, 2, 1, 5, 7, 1, 2, 2, 6, 2, 4, 3, 1, 2, 1, 0, 6, 2, 1, 0, 1]

    # your code here
    data = {'country_name':Series(countries),'gold':Series(gold),'silver':Series(silver),'bronze':Series(bronze)}
    olympic_medal_counts_df = DataFrame(data,index=countries)

    return olympic_medal_counts_df

```

# 索引数据框

```
import pandas as pd

'''
You can think of a DataFrame as a group of Series that share an index.
This makes it easy to select specific columns that you want from the 
DataFrame. 

Also a couple pointers:
1) Selecting a single column from the DataFrame will return a Series
2) Selecting multiple columns from the DataFrame will return a DataFrame

*This playground is inspired by Greg Reda's post on Intro to Pandas Data Structures:
http://www.gregreda.com/2013/10/26/intro-to-pandas-data-structures/
'''
# Change False to True to see Series indexing in action
if True:
    data = {'year': [2010, 2011, 2012, 2011, 2012, 2010, 2011, 2012],
            'team': ['Bears', 'Bears', 'Bears', 'Packers', 'Packers', 'Lions',
                     'Lions', 'Lions'],
            'wins': [11, 8, 10, 15, 11, 6, 10, 4],
            'losses': [5, 8, 6, 1, 5, 10, 6, 12]}
    football = pd.DataFrame(data)
    print football['year']
    print ''
    print football.year  # shorthand for football['year']
    print ''
    print football[['year', 'wins', 'losses']]

'''
Row selection can be done through multiple ways.

Some of the basic and common methods are:
   1) Slicing
   2) An individual index (through the functions iloc or loc)
   3) Boolean indexing

You can also combine multiple selection requirements through boolean
operators like & (and) or | (or)
'''
# Change False to True to see boolean indexing in action
if True:
    data = {'year': [2010, 2011, 2012, 2011, 2012, 2010, 2011, 2012],
            'team': ['Bears', 'Bears', 'Bears', 'Packers', 'Packers', 'Lions',
                     'Lions', 'Lions'],
            'wins': [11, 8, 10, 15, 11, 6, 10, 4],
            'losses': [5, 8, 6, 1, 5, 10, 6, 12]}
    football = pd.DataFrame(data)
    print football.iloc[[0]]
    print ""
    print football.loc[[0]]
    print ""
    print football[3:5]
    print ""
    print football[football.wins > 10]
    print ""
    print football[(football.wins > 10) & (football.team == "Packers")]

```


# 向量化方法

```
from pandas import DataFrame, Series
import numpy


def avg_medal_count():
    '''
    Compute the average number of bronze medals earned by countries who 
    earned at least one gold medal.  
    
    Save this to a variable named avg_bronze_at_least_one_gold. You do not
    need to call the function in your code when running it in the browser -
    the grader will do that automatically when you submit or test it.
    
    HINT-1:
    You can retrieve all of the values of a Pandas column from a 
    data frame, "df", as follows:
    df['column_name']
    
    HINT-2:
    The numpy.mean function can accept as an argument a single
    Pandas column. 
    
    For example, numpy.mean(df["col_name"]) would return the 
    mean of the values located in "col_name" of a dataframe df.
    '''


    countries = ['Russian Fed.', 'Norway', 'Canada', 'United States',
                 'Netherlands', 'Germany', 'Switzerland', 'Belarus',
                 'Austria', 'France', 'Poland', 'China', 'Korea', 
                 'Sweden', 'Czech Republic', 'Slovenia', 'Japan',
                 'Finland', 'Great Britain', 'Ukraine', 'Slovakia',
                 'Italy', 'Latvia', 'Australia', 'Croatia', 'Kazakhstan']

    gold = [13, 11, 10, 9, 8, 8, 6, 5, 4, 4, 4, 3, 3, 2, 2, 2, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0]
    silver = [11, 5, 10, 7, 7, 6, 3, 0, 8, 4, 1, 4, 3, 7, 4, 2, 4, 3, 1, 0, 0, 2, 2, 2, 1, 0]
    bronze = [9, 10, 5, 12, 9, 5, 2, 1, 5, 7, 1, 2, 2, 6, 2, 4, 3, 1, 2, 1, 0, 6, 2, 1, 0, 1]
    
    olympic_medal_counts = {'country_name':Series(countries),
                            'gold': Series(gold),
                            'silver': Series(silver),
                            'bronze': Series(bronze)}
    df = DataFrame(olympic_medal_counts)
    
    # YOUR CODE HERE
    broze_at_least_one_gold = df['bronze'][df['gold'] >= 1]
    avg_bronze_at_least_one_gold = numpy.mean(broze_at_least_one_gold)

    return avg_bronze_at_least_one_gold
```

```
import numpy
from pandas import DataFrame, Series


def avg_medal_count():
    '''
    Using the dataframe's apply method, create a new Series called 
    avg_medal_count that indicates the average number of gold, silver,
    and bronze medals earned amongst countries who earned at 
    least one medal of any kind at the 2014 Sochi olympics.  Note that
    the countries list already only includes countries that have earned
    at least one medal. No additional filtering is necessary.
    
    You do not need to call the function in your code when running it in the
    browser - the grader will do that automatically when you submit or test it.
    '''

    countries = ['Russian Fed.', 'Norway', 'Canada', 'United States',
                 'Netherlands', 'Germany', 'Switzerland', 'Belarus',
                 'Austria', 'France', 'Poland', 'China', 'Korea', 
                 'Sweden', 'Czech Republic', 'Slovenia', 'Japan',
                 'Finland', 'Great Britain', 'Ukraine', 'Slovakia',
                 'Italy', 'Latvia', 'Australia', 'Croatia', 'Kazakhstan']

    gold = [13, 11, 10, 9, 8, 8, 6, 5, 4, 4, 4, 3, 3, 2, 2, 2, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0]
    silver = [11, 5, 10, 7, 7, 6, 3, 0, 8, 4, 1, 4, 3, 7, 4, 2, 4, 3, 1, 0, 0, 2, 2, 2, 1, 0]
    bronze = [9, 10, 5, 12, 9, 5, 2, 1, 5, 7, 1, 2, 2, 6, 2, 4, 3, 1, 2, 1, 0, 6, 2, 1, 0, 1]
    
    olympic_medal_counts = {'country_name':countries,
                            'gold': Series(gold),
                            'silver': Series(silver),
                            'bronze': Series(bronze)}    
    df = DataFrame(olympic_medal_counts)
    
    # YOUR CODE HERE
    avg_medal_count = df[['gold','silver','bronze']].apply(numpy.mean)
    
    return avg_medal_count

```


# 矩阵乘法和Numpy Dot
```
import numpy
from pandas import DataFrame, Series


def numpy_dot():
    '''
    Imagine a point system in which each country is awarded 4 points for each
    gold medal,  2 points for each silver medal, and one point for each 
    bronze medal.  

    Using the numpy.dot function, create a new dataframe called 
    'olympic_points_df' that includes:
        a) a column called 'country_name' with the country name
        b) a column called 'points' with the total number of points the country
           earned at the Sochi olympics.
           
    You do not need to call the function in your code when running it in the
    browser - the grader will do that automatically when you submit or test it.
    '''

    countries = ['Russian Fed.', 'Norway', 'Canada', 'United States',
                 'Netherlands', 'Germany', 'Switzerland', 'Belarus',
                 'Austria', 'France', 'Poland', 'China', 'Korea', 
                 'Sweden', 'Czech Republic', 'Slovenia', 'Japan',
                 'Finland', 'Great Britain', 'Ukraine', 'Slovakia',
                 'Italy', 'Latvia', 'Australia', 'Croatia', 'Kazakhstan']

    gold = [13, 11, 10, 9, 8, 8, 6, 5, 4, 4, 4, 3, 3, 2, 2, 2, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0]
    silver = [11, 5, 10, 7, 7, 6, 3, 0, 8, 4, 1, 4, 3, 7, 4, 2, 4, 3, 1, 0, 0, 2, 2, 2, 1, 0]
    bronze = [9, 10, 5, 12, 9, 5, 2, 1, 5, 7, 1, 2, 2, 6, 2, 4, 3, 1, 2, 1, 0, 6, 2, 1, 0, 1]
 
    # YOUR CODE HERE
    data = {'country_name':Series(countries),'gold':Series(gold),'silver':Series(silver),'bronze':Series(bronze)}
    df = DataFrame(data)
    medal_counts = df[['gold','silver','bronze']]
    points = numpy.dot(medal_counts,[4,2,1])
    olympic_points = {'country_name': Series(countries),'points':Series(points)}
    olympic_points_df = DataFrame(olympic_points)
    
    return olympic_points_df
```

# 书签
Pandas文档
http://pandas.pydata.org/pandas-docs/stable/

Pandas IPython Notebook 教程
https://bitbucket.org/hrojas/learn-pandas

numpy.dot — NumPy v1.12 Manual
https://docs.scipy.org/doc/numpy/reference/generated/numpy.dot.html
