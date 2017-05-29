# Pandas-J
**读取csv**  

```python
pf = pd.read_csv(PATH, skiprows = 1, header = None,names= ['day'],usecols = ['Customer_Code'])
```

**读取table**  

```python
BORM = pd.read_table('input/BORM/TSBORM.P00028.csv',sep = ',',usecols=usedata_BORM,encoding="utf-8")
```

**显示内容的汇总**  

```python
BORM['STAT'].value_counts()
Merge_Rating.count()
```

**apply的用法**  

```python
CCMS['Customer_Code'] = CCMS['Customer_Code'].apply(Clean_CCM)
CCMS['Customer_Code'] = CCMS.apply(Clean_CCM,axis=1)
#axis=1 行   axis=0 列
```

**merge表合并**

```python
test= pd.merge(a, b, left_on = 'w', right_on = 'd', how = 'left')
```

**定位替换**

```pyrhon
a['w'].fillna('NULL',inplace=True)
a.loc[a['w']=='NULL','d']= False
a = BORM_ECL.drop(BORM_ECL[BORM_ECL['ECL_LT']<0].index)
```

**去重复**

```python
test=a.drop_duplicates(['KEY_1','CUSTOMER_NO','ACT_TYPE','Approve_Date'])
```

**删除列**

```python
Merge_Rating = Merge_Rating.drop(['Rating_Exp_Date_x'],axis=1)
```

**输出csv**

```python
BORM_MERGE.to_csv('input/BORM_MERGE/TSBORM.P00001.csv')
```

**excel时间转换/某个时间加上天数**

```pyrhon
pd.to_datetime('1900-01-01') + timedelta(days=int(x))     from datetime import timedelta
a = (pd.to_datetime('2015-12-31') - BORM['LST_UNPD_DUE_DTE'][0])
#a.days为取天数
```

**转换为dict**

```python
pd_r.to_dict('records')
```

**更换index**

```python
BLDVNN.index = BLDVNN['KEY_1']
```

# Pandas-R
jupyter notebook

**iat**  
  postion  
**按行**    
  axis=1  
**输出csv的时候不输出索引**  
  index=False  

**创建**  

```python
s = pd.Series([1,3,5,np.nan,6,8])
```  
**时间range**

```python
dates = pd.date_range('20130101', periods=6)
```
**dates为列 行为ABCD np random 6列4行的数据 **

```python
df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=list('ABCD'))
```  

**查看df的列**  

```python
df.index
```
**查看df的行** 

```python  
df.columns  
```
**查看df的值**  

```python
df.values  
```
**df的各种运算**

```python 
df.describe() 
``` 
**df的转梽** 

```python 
df.T  
```
**按照column id 排序 ascending=False为降序**  

```python
df.sort_index(axis=1, ascending=False)
```
**取A列**  

```python
df['A']  
```
**取前3列**  

```python
df[0:3] 
``` 
**取'20130102'到'20130104'**  

```python
df['20130102':'20130104']   
```
**取第一个date的那行数据**  
  
```python
df.loc[dates[0]]  
```
**取A,B列的数据**  

```python
df.loc[:,['A','B']]  
```
**取02月到04月的A,B列数据**   

```python
df.loc['20130102':'20130104',['A','B']]  
```
**取02月A,B列数据**  

```python
df.loc['20130102',['A','B']]  
```
**取第一个月的A列数据**  

```python
df.loc[dates[0],'A']  
```
**取第4行的值**   
```python
df.iloc[3]  
```
**取第4第5行，第1第2列**  

```python
df.iloc[3:5,0:2]  
```
**取2，3，5行，第1第2列**     

```python
df.iloc[[1,2,4],[0,2]]  
```
**取第2，3行**  

```python
df.iloc[1:3,:]  
```
**取第2，3列**

```python
df.iloc[:,1:3]  
```
**取第2行第2列**   

```python
df.iloc[1,1]
```  
**取A列大于0的行数据**   

```python
df[df.A > 0]  
```
**取大于0的所有数据**  

```python
df[df > 0]  
```

**df2复制于df**  

```python
df2 = df.copy()  
```
**给E列重新赋值**  

```python
df2['E'] = ['one', 'one','two','three','four','three']  
```
**取E列里['two','four']的所有行**  

```python
df2[df2['E'].isin(['two','four'])]
```

**从20130102开始创建6列数据，六列对应的值是[1,2,3,4,5,6]**  

```pyrhon
s1 = pd.Series([1,2,3,4,5,6], index=pd.date_range('20130102', periods=6))
```

**给df表的[dates[0],'A']赋值为0** 

```python
df.at[dates[0],'A'] = 0
```
**df的F列为s1**

```python
df['F'] = s1
```
**df的第3行第2列为0** 

```python
df.iat[0,1] = 0
```
**df的D列赋值为np.array([5] * len(df)) ! 把5输出6遍**

```python
df.loc[:,'D'] = np.array([5] * len(df))
```
**df2中大于0的取反 这样df2的大于0的值全为负**

```python
df2[df2 > 0] = -df2
```

**生成一个新的df1的表，第二行不为null**  

```python
df1 = df.reindex(index=dates[0:4], columns=list(df.columns) + ['E'])
df1.loc[dates[0]:dates[1],'E'] = 1
```
**去除df1中含null的行** 
 
```python
df1.dropna(how='any')
```
**把df1中null的值赋值为5**  
 
```python
df1.fillna(value=5)
```
**df1中如果是null的话 这个值为True,其余为False**

```python
pd.isnull(df1)
```
**求df每一列的平均数**

```python
df.mean()
```
**求第一行的平均数null不算** 

```python
df.mean(axis=1)
```
**对df的A列全部加1**

```python
df['A'] = df['A'].apply(lambda x:x+1)
```

**对value的值进行统计** 

```python
s = pd.Series(np.random.randint(0, 7, size=10)) 
s.value_counts()
```

**把s表中的value大学转化为小写**

```python
s = pd.Series(['A', 'B', 'C', 'Aaba', 'Baca', np.nan, 'CABA', 'dog', 'cat'])
s.str.lower()
```

**把一个数组的所有的DataFrame拼接成一个大的DataFrame**

```python
BORM = pd.concat(results)
```

**left 和 right 按照key列为主键进行合并 **

```python
left = pd.DataFrame({'key': ['foo', 'foo'], 'lval': [1, 2]})
right = pd.DataFrame({'key': ['foo', 'foo'], 'rval': [4, 5]})
pd.merge(left, right, on='key')
```

**df后面加上s数列(s为一个DataFrame)**

```python
df = pd.DataFrame(np.random.randn(8, 4), columns=['A','B','C','D'])
s = df.iloc[3]
df.append(s, ignore_index=True)
```



**根据df的列A,B进行groupby cd列数字求和**

```python
df = pd.DataFrame({'A' : ['foo', 'bar', 'foo', 'bar','foo', 'bar', 'foo', 'foo'],
                   'B' : ['one', 'one', 'two', 'three','two', 'two', 'one', 'three'],
                   'C' : np.random.randn(8),
                   'D' : np.random.randn(8)})
df.groupby(['A','B']).sum()
```



```python
tuples = list(zip(*[['bar', 'bar', 'baz', 'baz',
'foo', 'foo', 'qux', 'qux'],
['one', 'two', 'one', 'two',
'one', 'two', 'one', 'two']]))
```


```python
index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])
df = pd.DataFrame(np.random.randn(8, 2), index=index, columns=['A', 'B'])

```

**生产n*m全零矩阵**

```python
np.zeros((n,m))
```

