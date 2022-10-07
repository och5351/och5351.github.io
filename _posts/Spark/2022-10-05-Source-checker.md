---
title: "Source checker"
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

* 프로그램 명 : source_checker
* 목적 : Source의 테이블 상태를 확인 하기 위해 만드는 테이블

<br><br>

# 사용법

<br>

조회할 테이블 리스트, 스키마 리스트를 변수에 넣어 iter_checker를 실행시킨다.

<br><br>


<br><br>

```python
import numpy as np
import pandas as pd
from lib.DB_Connector_REL.db_controller import EXA_DW_Connector # DW 오라클 엑사 DB Connection 호출 
import os

ora_exa = EXA_DW_Connector()
ora_exa.connect(ora_client_path = r'D:\app\client\Siltron\product\19.0.0\client_1\bin')

def partition_checker(table:str,
                schema:str)->str:

        sql = f"SELECT SEGMENT_TYPE\
                FROM dba_segments\
                WHERE SEGMENT_NAME = '{table}'\
                AND OWNER = '{schema}'\
                GROUP BY SEGMENT_TYPE"
        try:
                return pd.read_sql(sql=sql, con=ora_exa.conn)['SEGMENT_TYPE'].values[0]
        except:
                return ''

def size_checker(table:str,
                schema:str)->str:

        sql = f"SELECT sum((bytes/1024/1024/1024)) AS TABLE_SIZE_GB\
                FROM dba_segments\
                WHERE SEGMENT_NAME = '{table}'\
                AND OWNER = '{schema}'"

        return pd.read_sql(sql=sql, con=ora_exa.conn)['TABLE_SIZE_GB'].values[0]

def cmt_checker(table:str,
                schema:str)->str:
        
        sql = f"SELECT *\
                FROM ALL_TAB_COMMENTS\
                WHERE TABLE_NAME = '{table}'\
                AND OWNER = '{schema}'"
        try:
                return pd.read_sql(sql=sql, con=ora_exa.conn)['COMMENTS'].values[0]
        except:
                return ''

def row_counter(table:str,
                schema:str)->str:
        sql = f"SELECT NUM_ROWS\
                FROM DBA_TABLES\
                WHERE TABLE_NAME = '{table}'\
                AND OWNER = '{schema}'"
        try:
                return pd.read_sql(sql=sql, con=ora_exa.conn)['NUM_ROWS'].values[0]
        except:
                return ''

def col_counter(table:str,
                schema:str)->str:
        
        sql = f"SELECT COUNT(*) AS CNT\
                FROM ALL_TAB_COLUMNS\
                WHERE TABLE_NAME = '{table}'\
                AND OWNER = '{schema}'"
        try:
                return pd.read_sql(sql=sql, con=ora_exa.conn)['CNT'].values[0]
        except:
                return ''

def partition_key_checker(table:str,
                        schema:str)->str:

        sql = f"SELECT COLUMN_NAME\
                FROM ALL_PART_KEY_COLUMNS\
                WHERE NAME = '{table}'\
                AND OWNER = '{schema}'"

        meta = pd.read_sql(sql=sql, con=ora_exa.conn)

        val = ', '.join(meta.COLUMN_NAME.values)

        return None if val == '' else val

def primary_key_checker(table:str,
                        schema:str)->np.array:

        sql = f"SELECT a.table_name\
                , a.index_name\
                , a.column_name\
                , b.comments\
                FROM all_ind_columns a\
                , all_col_comments b\
                WHERE a.table_name = '{table}'\
                AND a.table_owner = '{schema}'\
                AND b.OWNER = '{schema}'\
                AND a.table_name = b.table_name\
                AND a.column_name = b.column_name\
                AND a.index_name LIKE '%PK'\
                ORDER BY a.index_name\
                , a.column_position"

        pk_check = pd.read_sql(sql=sql, con=ora_exa.conn)
        val = ', '.join(pk_check['COLUMN_NAME'].values)
        return None if val == '' else val 

def iter_checker(table_list:list, schema_list:list, format_list:list)->pd.DataFrame:
        data = np.array([])
        for table, schema, format_ in zip(table_list,schema_list,format_list):

                seg_type = partition_checker(table=table, schema=schema)
                seg_type = 'VIEW' if seg_type == '' else seg_type
                gb_size = size_checker(table=table, schema=schema)
                cmt = cmt_checker(table=table, schema=schema)
                pt_key = partition_key_checker(table=table, schema=schema)
                pk = primary_key_checker(table=table, schema=schema)
                row_cnt = row_counter(table=table, schema=schema)
                col_cnt = col_counter(table=table, schema=schema)
                data = np.append(data , np.array([schema, table, table, cmt, seg_type, pt_key, pk, gb_size, 'GB', format_, row_cnt, col_cnt]))

        columns = ['Schema Name','Table Name', 'T_Table Name', 'Explain', 'Category','Partition Key', 'Primary Key', 'Capability', 'Unit', 'Target Format', 'Row','Col']

        return pd.DataFrame(data.reshape(len(table_list),len(columns)), columns=columns)


batch_name = 'ET_S_EXA-CDP_D_08_12'

# 03 ~ 07
TB_LIST = ['USER.TABLE_NAME']

table_list = []
schema_list = []
format_list = []
for table in TB_LIST:
    schem, t_name = table.split('.')
    table_list.append(t_name)
    schema_list.append(schem)
    format_list.append('parquet')


df = iter_checker(table_list=table_list, schema_list=schema_list, format_list=format_list)

encode = 'euc-kr'

os.makedirs('../Sqoop_generator_DEV/SRC_META_CSV/', exist_ok=True)
os.makedirs('../Hive_DDL_generator_DEV/SRC_META_CSV/', exist_ok=True)
os.makedirs('../Impala_generator_DEV/SRC_META_CSV/', exist_ok=True)
os.makedirs('../Kudu_DDL_generator_DEV/SRC_META_CSV/', exist_ok=True)
os.makedirs('../Count_validation_DEV/SRC_META_CSV/', exist_ok=True)
os.makedirs('./SRC_META_CSV/', exist_ok=True)

df.to_csv(f'../Sqoop_generator_DEV/SRC_META_CSV/{batch_name}.csv', encoding=encode, index=False)
df.to_csv(f'../Hive_DDL_generator_DEV/SRC_META_CSV/{batch_name}.csv', encoding=encode, index=False)
df.to_csv(f'../Impala_generator_DEV/SRC_META_CSV/{batch_name}.csv', encoding=encode, index=False)
df.to_csv(f'../Kudu_DDL_generator_DEV/SRC_META_CSV/{batch_name}.csv', encoding=encode, index=False)
df.to_csv(f'../Count_validation_DEV/SRC_META_CSV/{batch_name}.csv', encoding=encode, index=False)
df.to_csv(f'./SRC_META_CSV/{batch_name}.csv', encoding=encode, index=False)

df

```

<br><br>