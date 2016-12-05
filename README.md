# ML_In_Action

Sklearn Matplotlib seaborn Numpy Pandas Numpy Scipy

```python
>>> from numpy import *
>>> random.rand(4,4)  # array
>>> randMat = mat(random.rand(4,4))
>>> randMat.I # inverse of a matrix
>>> eye(4)
```

## K-Nearest Neighbors

- Pros: High accuracy, insensitive to outliers, no assumptions about data
- Cons: Computationally expensive, requires a lot of memory
- Works with: Numeric values, nominal values

### Geneeral appraoch to kNN
1. Collect: Any method.
2. Prepare: Numeric values are need for a distance calculation. A structured data format is best.
3. Analyze: Any method.
4. Train: Does not apply to the kNN algorithm.
5. Test: Calculate the error rate.
6. Use: This application needs to get some input data and output structured numeric values. Next, the application runs the kNN algorithm on this input data and determines which class the input data should belong to. The application then takes some action on the calculated class


normalizing numeric values
```python
newValue = (oldValue-min)/(max-min)
```
