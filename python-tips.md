# download course material 

1. install coursea-dl.  

pip install cooursera-dl

2. open  https://learner.coursera.help/hc/en-us login in with your account info

3. wait for about 1-2 minutes runing command to download all material

coursera-dl -u username -p password course_name

# pandas tips with large data

https://towardsdatascience.com/why-and-how-to-use-pandas-with-large-data-9594dda2ea4c

large data means < 100GB 
1.  Read CSV file data in chunk size
'''
# read the large csv file with specified chunksize 
df_chunk = pd.read_csv(r'../input/data.csv', chunksize=1000000)
'''

2. 2. Filter out unimportant columns to save memory

'''
# Filter out unimportant columns
df = df[['col_1','col_2', 'col_3', 'col_4', 'col_5', 'col_6','col_7', 'col_8', 'col_9', 'col_10']]
'''
3. Change dtypes for columns
I can say that changing data types in Pandas is extremely helpful to save memory, especially if you have large data for intense analysis or computation (For example, feed data into your machine learning model for training).

'''
# Change the dtypes (int64 -> int32)
df[['col_1','col_2', 
    'col_3', 'col_4', 'col_5']] = df[['col_1','col_2', 
                                      'col_3', 'col_4', 'col_5']].astype('int32')

# Change the dtypes (float64 -> float32)
df[['col_6', 'col_7',
    'col_8', 'col_9', 'col_10']] = df[['col_6', 'col_7',
                                       'col_8', 'col_9', 'col_10']].astype('float32
'''

# python += vs append

https://stackoverflow.com/questions/2022031/python-append-vs-operator-on-lists-why-do-these-give-different-results

c = [1, 2, 3]
c += c 
results is [1,2,3,1,2,3]

c = [1, 2, 3]
c.append(c)
results is [1, 2, 3, [...]]



# run python code with conda env directly from windows task schedule

1. install anaconda for example we install at C:\Anaconda3
2. create the env as my_env
3. install all packages you need
4. write your python code save as my_test.py
5. create a bat file  my_job.bat and input below code in the bat file
```
call activate  my_env
python D:\work\test\my_test.py -c D:\work\test\test_confg.yaml
call deactivate
```
6. set window envirment variable: set path variables C:\Anaconda3;C:\Anaconda3\Scripts;

7. open window search and search task scheduler and open it. On the right, create a task.

8. On the 'Triggers' tab, set when you want your code run

9. on the 'Actions' tab in the Program/script: input your bat file path. In this example is D:\work\test\my_job.bat

done!


1. Use the rename function and refer the columns to be renamed. Not all the columns have to be renamed:
'df = df.rename(columns={'oldName1': 'newName1', 'oldName2': 'newName2'})'
OR
'df.rename(columns={'oldName1': 'newName1', 'oldName2': 'newName2'}, inplace=True)'


2. Pandas find rows in A not in B
https://stackoverflow.com/questions/32652718/pandas-find-rows-which-dont-exist-in-another-dataframe-by-multiple-columns
Since 0.17.0 there is a new indicator param you can pass to merge which will tell you whether the rows are only present in left, right or both:
merged = df.merge(other, how='left', indicator=True)
merged

Out[5]:
   col1 col2  extra_col     _merge
0     0    a       this  left_only
1     1    b         is       both
2     1    c       just  left_only
3     2    b  something  left_only

In [6]:    
merged[merged['_merge']=='left_only']

Out[6]:
   col1 col2  extra_col     _merge
0     0    a       this  left_only
2     1    c       just  left_only
3     2    b  something  left_only
So you can now filter the merged df by selecting only 'left_only' rows



3. package 

在计算机程序的开发过程中，随着程序代码越写越多，在一个文件里代码就会越来越长，越来越不容易维护。

为了编写可维护的代码，我们把很多函数分组，分别放到不同的文件里，这样，每个文件包含的代码就相对较少，很多编程语言都采用这种组织代码的方式。在Python中，一个.py文件就称之为一个模块（Module）。

使用模块有什么好处？

最大的好处是大大提高了代码的可维护性。其次，编写代码不必从零开始。当一个模块编写完毕，就可以被其他地方引用。我们在编写程序的时候，也经常引用其他模块，包括Python内置的模块和来自第三方的模块。

使用模块还可以避免函数名和变量名冲突。相同名字的函数和变量完全可以分别存在不同的模块中，因此，我们自己在编写模块时，不必考虑名字会与其他模块冲突。但是也要注意，尽量不要与内置函数名字冲突。点这里查看Python的所有内置函数。

你也许还想到，如果不同的人编写的模块名相同怎么办？为了避免模块名冲突，Python又引入了按目录来组织模块的方法，称为包（Package）。

举个例子，一个abc.py的文件就是一个名字叫abc的模块，一个xyz.py的文件就是一个名字叫xyz的模块。

现在，假设我们的abc和xyz这两个模块名字与其他模块冲突了，于是我们可以通过包来组织模块，避免冲突。方法是选择一个顶层包名，比如mycompany，按照如下目录存放：

![example](0.png "example")

mycompany

引入了包以后，只要顶层的包名不与别人冲突，那所有模块都不会与别人冲突。现在，abc.py模块的名字就变成了mycompany.abc，类似的，xyz.py的模块名变成了mycompany.xyz。

请注意，每一个包目录下面都会有一个__init__.py的文件，这个文件是必须存在的，否则，Python就把这个目录当成普通目录，而不是一个包。__init__.py可以是空文件，也可以有Python代码，因为__init__.py本身就是一个模块，而它的模块名就是mycompany。

类似的，可以有多级目录，组成多级层次的包结构。比如如下的目录结构：

![example](https://github.com/handongfeng/researchNotes/blob/master/0%20(1).png "example")

mycompany-web

文件www.py的模块名就是mycompany.web.www，两个文件utils.py的模块名分别是mycompany.utils和mycompany.web.utils。

mycompany.web也是一个模块，请指出该模块对应的.py文件。



#

If you are on Windows, install the latest version of the Microsoft ODBC Driver for SQL Server to ensure the DATE and TIME types are supported.

If you are on Linux or macOS, use this page to determine the latest driver available for your distribution.

Use parameter placeholders and pass the values as date and time objects for the current datetime value.
```
now = datetime.datetime.now()
sql = "insert into Database3([row name], [my date], [my time]) values (?,?,?)"
cursor.execute(sql, ('new1', now.date(), now.time()))
cnxn.commit()
```
