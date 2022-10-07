---
title: "Sum Validation"
toc: true
# header:
#   image: /assets/images/spark/spark_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/f3/Apache_Spark_logo.svg)"
layout: posts
categories:
  - Spark
tags:
  - Spark
toc: true
toc_sticky: true

date: 2022-10-05
last_modified_at: 2022-10-05
---

<br><br>

# 설명

<br>

하이브와 소스의 숫자열의 합을 비교하는 프로그램.

<br><br>

# 사용법

<br>


<br><br>


<br><br>

```python
%pyspark

# oracle connector
def ora_select(sql:str) -> spark:
    return spark.read \
                .format("jdbc") \
                .option("url", "jdbc:oracle:thin:@IP:PORT:SID") \
                .option("user", "USERID").option("password", "PASSWORD") \
                .option("query", sql) \
                .load()
        

# oracle number type collector
def ora_num_type_collector(table_name:str, 
                            schema_name:str) -> spark:
    
    sql = f"SELECT COLUMN_NAME \
    FROM ALL_TAB_COLUMNS \
    WHERE TABLE_NAME = '{table_name.upper()}' \
    AND OWNER = '{schema_name.upper()}' \
    AND DATA_TYPE LIKE 'NUMBER%'"
    
    return list(map(lambda x : x[0], ora_select(sql).collect()))

# oracle sum select
def ora_sum_select(col_name:list, 
                    table_name:str, 
                    schema_name:str) -> spark:
    
    sql = f"SELECT {','.join(col_name)} \
    FROM {schema_name.upper()}.{table_name.upper()}"
    
    return ora_select(sql)

# oracle partition sum select
def ora_part_sum_select(col_name:list, 
                        table_name:str, 
                        schema_name:str,
                        pt_key:str,
                        start_date:str,
                        end_date:str) -> spark:
    
    sql = f"SELECT {','.join(col_name)} \
    FROM {schema_name.upper()}.{table_name.upper()} \
    WHERE {pt_key} >= '{start_date}' \
    AND {pt_key} <= '{end_date}'"
    
    return ora_select(sql)

# hive sum select
def hive_sum_select(col_name:list, 
                    table_name:str, 
                    schema_name:str) -> spark:
    hql = f"SELECT {','.join(col_name)} \
    FROM {schema_name}.{table_name}"
    
    return spark.sql(hql)

# hive pratition sum select
def hive_part_sum_select(col_name:list, 
                    table_name:str, 
                    schema_name:str,
                    pt_key:str,
                    start_date:str,
                    end_date:str) -> spark:
                        
    hql = f"SELECT {','.join(col_name)} \
    FROM {schema_name}.{table_name} \
    WHERE  {pt_key} >= '{start_date}' \
    AND {pt_key} <= '{end_date}'"
    
    return spark.sql(hql)

# oracle partition check
def ora_partition(table_name:str,
                schema_name:str)->str:

    sql = f"SELECT COLUMN_NAME\
            FROM ALL_PART_KEY_COLUMNS\
            WHERE NAME = '{table_name.upper()}'\
            AND OWNER = '{schema_name.upper()}'"
            
    pt_k_meta = ora_select(sql)
    
    return list(map(lambda x : x[0], pt_k_meta.collect()))

# Compare Data
def compare_sum_val(df_1:spark.sql,
                    df_2:spark.sql,
                    col_name:list)-> None:
    
    
    for col in col_name:
    
        comp1 = df_1.select(col).collect()[0]
        comp2 = df_2.select(col).collect()[0]
        
        status = 'O' if comp1 == comp2 else 'X'
        print(f"{status} \t", end='')
        print('oracle : ', comp1 , '\t', 'hive : ', comp2 , '\n')
    
    return
    

# Table & Partition 분류
'''
    * pt_key 기준 Branch
    * Partition 경우 start, end date 변수
'''
def tb_branch(col_name:str,
            table_name:str,
            schema_name:str,
            lake_schema_name:str,
            start_date:str = '202210',
            end_date:str = '202211'
            )->str:
                
    pt_key = ora_partition(table_name=table_name, schema_name=schema_name)
    
    sum_col_name = list(map(lambda x : ''.join(['SUM(',x,') AS ' , x ]), col_name))
    
    
    hive_df:spark.sql
    oracle_df:spark.sql
    
    if pt_key:
        if pt_key[0].upper() != 'BASE_DT':
            start_date = ''.join([start_date, '01070000000000'])
            end_date = ''.join([end_date, '01070000000000'])
            
        hive_df = hive_part_sum_select(col_name=sum_col_name, 
                            table_name=table_name, 
                            schema_name=lake_schema_name,
                            pt_key = pt_key[0],
                            start_date = start_date,
                            end_date = end_date)
                          
        oracle_df = ora_part_sum_select(col_name=sum_col_name, 
                            table_name=table_name,
                            schema_name=schema_name,
                            pt_key = pt_key[0],
                            start_date = start_date,
                            end_date = end_date)
                                
    else:
        hive_df = hive_sum_select(col_name=sum_col_name, 
                            table_name=table_name, 
                            schema_name=lake_schema_name)

        oracle_df = ora_sum_select(col_name=sum_col_name, 
                            table_name=table_name, 
                            schema_name=lake_schema_name)
                            
    compare_sum_val(df_1 = hive_df,
                    df_2 = oracle_df,
                    col_name = col_name)


# ======================================= main =======================================

table_name = 'TABLE_NAME'
schema_name = 'USER_NAME'
lake_schema_name = 'HADOOP_MANAGED_PATH'
start_date = '202010'
end_date = '202209'


data_type_lst = ora_num_type_collector(table_name=table_name, 
                                        schema_name=schema_name)


tb_branch(col_name=data_type_lst,
            table_name=table_name,
            schema_name=schema_name,
            lake_schema_name = lake_schema_name,
            start_date = start_date,
            end_date = end_date)
```