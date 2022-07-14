# -python-Excel-
1.将两张表合并为一张表

2.找出其中身份证号相同，工资不同的人（一张表中有两个相同的人，工资不同且工资相加）

3.将工资相加，把另一个身份证号相同的信息去掉，保存为一张新的表


#合并两张表

import pandas as pd

import os

path= r'D://test-e'   #两张表的读取、保存路径

excel_list = [f for f in os.listdir(path) if f.endswith('.xlsx')]

files = [os.path.join(path, f) for f in excel_list]

#合并两张表

for i in range(0,len(files)):

    data = pd.DataFrame(pd.read_excel(files[i]))
    
    data1 = data.loc[:,["ID","姓名","年龄","薪资"]]
    
    data1 = data1[data1["ID"].notnull()]
    
    if i==0:
    
        data_all = data1
        
    else:
    
        data_all = pd.concat([data_all,data1],axis=0)

data_all.to_excel(r'D://test-e//汇总.xlsx',index=False)    #为之后读取Excel时不会出现Unnamed的无用标签



#对两张表进行处理

import pandas as pd

df=pd.read_excel('D://test-e//汇总.xlsx')

df=df.loc[:,:]

print(df)

df_group=df.groupby(["ID","姓名","年龄"]).sum().loc[ :, : "薪资"]   #用分组的方式，将ID，姓名，年龄作为标签固定不变，然后对薪资进行加减运算。

print(df_group)    #输出检测

df_group.to_excel("D://test-e//汇总2.xlsx")   #保存表单

df2=pd.read_excel("D://test-e//汇总2.xlsx")   #对表单进行检验

print(df2)      #凭空出现一行0-n是pandas库默认的标签值，在保存时，只需将index=False即可。
