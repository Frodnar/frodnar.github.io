---
title: "Descritizing numerical columns for ML with pandas"
date: 2023-02-11T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["Programming", "ML", "Tips and Tricks", "Pandas", "Python"]
author: "Chris"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: true
draft: false 
hidemeta: false
comments: false
description: "A quick trick for one-hot encoding numerical columns"
canonicalURL: "https://frodnar.github.io/posts/2023-02-12_descritize_numerical_columns_pandas"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
cover:
    image: "images/df_descritize.png" # image path/url
    alt: "Screenshot of pandas dataframe" # alt text
    caption: "" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
editPost:
    URL: "https://github.com/Frodnar/frodnar.github.io/tree/main/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---
## Getting Started

I am a big fan of Python and its powerful data analysis library, pandas. Today, I'd like to share a trick that's save me tons of time when trying to one hot encode continuous numerical data in pandas.

One hot encoding is a technique used to convert categorical variables into 1s and 0s in order to prepare data for machine learning algorithms. Normally, this is done using the `get_dummies()` function in pandas. However, what if we want to one hot encode continuous numerical data using binning?

## The code

The solution is to chain the `get_dummies()` function with `cut` or `qcut`. This allows us to one hot encode the resulting binned data and thus convert the continuous numerical values into one hot encoded categories. Here's how we can do it:

    import pandas as pd
    import numpy as np

    # Create a sample dataframe with numerical data
    np.random.seed(0)
    df = pd.DataFrame({'col1': np.random.randint(0, 100, size=(100,))})

    # Discretize the numerical data into bins using the cut function
    df['col1_bins'] = pd.cut(df['col1'], bins=4)

    # One hot encode the resulting bins
    df = pd.get_dummies(df, columns=['col1_bins'])

    # Another way to do it would be using the qcut function:
    df['col1_bins'] = pd.qcut(df['col1'], q=4)
    df = pd.get_dummies(df, columns=['col1_bins'])

And that's it! Just as in the image at the top of this post, we've transformed our numerical data into one hot encoded categories that we can use in our machine learning models.  Note that you can also pass a custom list of bin edges to `cut` if needed.

## The context

Ok, great -- so we can discretize a continuous variable.  So what?  Why do it?  Well, there are several reasons why this is done:

1. Improved model interpretability: Discretization makes it easier to understand the relationship between the input variables and the target variable, as categorical variables can be easily visualized and analyzed.

2. Handling non-linear relationships: Discretization can capture non-linear relationships between the continuous variable and the target variable that might not be apparent in the original data.

3. Handling outliers: Discretization can help to reduce the impact of outliers on the model, as outliers in a continuous variable can have a large impact on the model's predictions.

4. Reduced computation time: Discretizing a continuous variable can reduce the size of the data and the complexity of the model, making computation faster and more efficient.

Despite these potential advantages, it's important to be careful when discretizing, as the process can result in information loss and can adversely affect the performance of the model if not done correctly.

## Wrapping up

I hope this trick is useful for you and that it helps you with your own data analysis projects. Pandas is definitely a useful library, and I'm always discovering new and interesting ways to use it. If you're interested in learning more about data analysis with Python and pandas, give me a [follow on LinkedIn](https://www.linkedin.com/in/christopher-j-roberts/). Happy coding!