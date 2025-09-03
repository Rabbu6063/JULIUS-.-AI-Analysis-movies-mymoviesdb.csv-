# Troubleshoot reading CSV by trying different engines and parameters, and preview raw bytes.
import pandas as pd
import chardet

# Detect encoding quickly
with open('mymoviedb.csv', 'rb') as f:
    raw = f.read(50000)
result = chardet.detect(raw)
print('Detected encoding:')
print(result)

# Try python engine with larger field size and on_bad_lines skip
try:
    df_try = pd.read_csv('mymoviedb.csv', engine='python', sep=',', encoding='utf-8', on_bad_lines='skip')
    print('Read with python engine, shape:')
    print(df_try.shape)
    print('Head:')
    print(df_try.head())
except Exception as e:
    print('Python engine read failed:')
    print(type(e))
    print(e)

# If columns look wrong, try specifying column names and quoting
col_names = ['Release_Date','Title','Overview','Popularity','Vote_Count','Vote_Average','Original_Language','Genre','Poster_Url']
try:
    df_try2 = pd.read_csv('mymoviedb.csv', engine='python', names=col_names, header=0, quoting=3, encoding='utf-8', on_bad_lines='skip')
    print('Read with specified columns, shape:')
    print(df_try2.shape)
    print('Head:')
    print(df_try2.head())
except Exception as e:
    print('Read with specified columns failed:')
    print(type(e))
    print(e)
