---
title: "Topic Modeling Arxiv Article Abstracts using NMF ... By ChatGPT"
date: 2023-02-20T11:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["Programming", "ML", "Unsupervised", "Project", "Python", "ChatGPT"]
author: "ChatGPT"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: true
draft: false 
hidemeta: false
comments: false
description: "An overview of using NMF for topic modeling written by a generative AI algorithm"
canonicalURL: "https://frodnar.github.io/posts/2023-02-20_nmf_on_arxiv_data_by_chatgpt"
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
    image: "images/plotly_scatter.png" # image path/url
    alt: "Screenshot of plotly scatter plot" # alt text
    caption: "" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
editPost:
    URL: "https://github.com/Frodnar/frodnar.github.io/tree/main/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---
## Getting Started

So I'll confess, I'm pretty late to the party here.  ChatGPT and other generative AI models have taken the popular conciousness by storm while I've been sitting around trying to ignore the hype.  I've seen previous advancements in generative audio and imagery models and shrugged at the relative lack of societally beneficial applications that surfaced.  I've even been uncomfortable using the term 'artificial intelligence' to refer to any system -- you'll notice it doesn't appear in any of my content -- but that may be starting to change. 

I am here to admit that I am utterly shocked at the performance *and utility* of the current state-of-the-art generative language models.  Are there potentially huge dangers in terms of bias, propoganda, and uneven distribution of benefits?   Yes, of course.  However, just because a tool is dangerous doesn't mean we shouldn't learn to use it carefully and make it better.  

I've seen loads of ChatGPT-generated text on social media lately, but what changed my mind was the ability to interact with ChatGPT around code and related concepts.  Perhaps if I had tried [GitHub Copilot](https://github.com/features/copilot) sooner, I wouldn't be acting such the fanboy right now.  In any case, I wanted to provide an example of runnable code and its explanation in order to (1) justify my fanboyism, and (2) because I haven't seen examples of complex code being generated in my social media feeds.

So, without further ado, here is the unedited reponse to a prompt that I gave ChatGPT to write an article with working code about a specific type of topic modeling called non-negative matrix factorization, or NMF, to be used on a corpus of paper abstracts from [arxiv.org](https://arxiv.org/).  I gave it specific instructions on which python libraries to use and how to visualize the results.

## Begin ChatGPT

In this article, we'll discuss how to analyze a corpus of academic articles using topic modeling and visualize the results as a scatter plot. Specifically, we'll use Python to read in a CSV file containing article titles and summaries, preprocess the text, and perform topic modeling using Non-negative Matrix Factorization (NMF). Finally, we'll use UMAP to position the articles in a scatter plot with colors representing topics.

To start, we need to import the necessary Python libraries:

    import pandas as pd
    import numpy as np
    import re
    import string
    from sklearn.feature_extraction.text import TfidfVectorizer
    from sklearn.decomposition import NMF
    import umap
    import plotly.express as px

Next, we'll read in the CSV file and concatenate the title and summary fields into a single text field:

    df = pd.read_csv('articles.csv')
    df['text'] = df['title'] + ' ' + df['summary']

We'll then preprocess the text by converting all text to lowercase, removing punctuation and digits, and removing stopwords. We'll use the NLTK library for this:

    import nltk
    nltk.download('stopwords')
    from nltk.corpus import stopwords

    stop_words = set(stopwords.words('english'))

    def preprocess_text(text):
        text = text.lower()
        text = re.sub(r'\d+', '', text)
        text = text.translate(str.maketrans('', '', string.punctuation))
        text = ' '.join([word for word in text.split() if word not in stop_words])
        return text

    df['processed_text'] = df['text'].apply(preprocess_text)

Now, we'll create a TF-IDF vectorizer and use it to transform the preprocessed text into a matrix of TF-IDF features:

    tfidf_vectorizer = TfidfVectorizer(max_df=0.95, min_df=2, max_features=1000, stop_words='english')
    tfidf = tfidf_vectorizer.fit_transform(df['processed_text'])

We'll then use NMF to perform topic modeling with 10 topics:

    nmf_model = NMF(n_components=10)
    nmf_features = nmf_model.fit_transform(tfidf)

Finally, we'll use UMAP to reduce the dimensionality of the NMF features and plot the articles as a scatter plot with colors representing topics:

    umap_model = umap.UMAP(n_neighbors=15, n_components=2, min_dist=0.1, metric='cosine')
    umap_embeddings = umap_model.fit_transform(nmf_features)

    topics = np.argmax(nmf_features, axis=1)

    fig = px.scatter(x=umap_embeddings[:,0], y=umap_embeddings[:,1], color=topics)
    fig.show()

In this scatter plot, each dot represents an article, and the colors represent the topic assigned by NMF. We can use this plot to explore the topics and their relationships to each other.

In conclusion, we've shown how to perform topic modeling on a corpus of academic articles using NMF and visualize the results as a scatter plot with UMAP. This analysis can be useful for exploring the topics and relationships within a corpus of text data.

## End ChatGPT

## Wrapping up

Now, if you're like me, you'll want to explore the plot that was generated.  I've hosted it [here](https://data-for-humans.com/wp-content/uploads/2023/02/chatgpt_generated_plot.html) for easy interaction.  Back to the main point, though.

I'm pretty floored by how well today's generative models can write and explain code.  There certainly are caveats and minor errors, and of course ChatGPT falls down whenever it's asked questions that are outside its training data. For instance, good luck getting working examples on more recently updated libraries that have major changes to their API or library structure since 2021.  As a former chemist, I can also confirm ChatGPT is ... not optimized ... for use in the chemical sciences.

But on balance, I'm still blown away by the ability of these large language models to generate sensible, well-documented, and somewhat efficient code and then turn around and explain the code that it generated.  Stitching together the outputs of several different libraries, even if they are commonly used together, is no simple task, and it would have taken me a couple of hours of research to throw together more poorly documented code to do the same thing.  

ChatGPT generated the above article with working code in about 30 seconds.
