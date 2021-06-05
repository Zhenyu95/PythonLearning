# Pandas

## To shuffle the df

```python
df = df.iloc[np.random.permutation(len(df))]
```

## Convert columns from numeric to Binary

```python
# if df['class'] == 2, change to 0, otherwise 1 
df['class'] = np.where(df['class'] == 2, 0, 1)
```



