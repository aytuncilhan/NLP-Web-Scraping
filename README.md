# Bugcrowd Platform Advertised Programs Analysis

## Step 1: Data Extraction
The site uses JSON format to read Programs data but in batches of size 25. Each batch is iteratively read and the results are written into [bugcrowd_Programs.csv](https://github.com/aytuncilhan/BugcrowdAnalysis/blob/master/bugcrowd_Programs.csv)

## Step 2 - 3: Data Analysis
BeautifulSoup package is used for HTML parsing and Pandas is used for data storage.

The “Program Details” are stored under a div with class name “bounty-content”.

Once obtained the raw text including program details, regular expressions and list operations are used to find dollar amounts (ruling out false-friends).

## Step 4: Results

In this step, the minimum and maximum dollar amounts obtained in step 3 are evaluated and a graph of the frequency of the minimum and maximum dollar amounts (histogram) is created. 

In doing so,
* Removed dataframe items which have $0 as both minimum and maximum values (since this means no bounty data was found for that program).
* The values are not rounded to full 1000s as it distored the dataset values: For instance most of the Min bounty values for rounded to 0 and hence ruled out... Albeit, the line to round values is still there, just commented out:
    `bounties = [round(num,-3) for num in bounties]`

There are two plots to generate: *Minimum Bounty Histogram* and *Maximum Bounty Histogram*. 

The plots looks nicer and more meaningful when the x-axis is plotted in log scale (since the values increase drastically even if they are all Min - or respectively all Max). However, both linear scale and log scale x-axis versions of the histograms are plotted and depicted below (In the script, I've added a scale parameter to a custom plot function to specify x-axis scale: `plotHistograms(minB, maxB, 'log')`)

Below are the histograms generated using matplotlib:
Log scale in x-axis | Linear scale in x-axis
------------ | -------------
![Plot](https://github.com/aytuncilhan/BugcrowdAnalysis/blob/a8021d809b4ec4694ebe38a05781df710e963a48/Plots/LogscaleHistograms.png) | ![Plot](https://github.com/aytuncilhan/BugcrowdAnalysis/blob/753011a530752bba6f056d5b246da596a73ec6f1/Plots/LinearscaleHistograms.png) 

## Step 5: Future Work

For further analysis, the following points can be considered:

### 1. Collocation Analysis

Collocation Analysis can be done using some "predefined words/phrases" to find out which words are mentioned together with these predefined words and hence obtain thematically similar blocks. To identify these "predefined words", we can use unsupervised machine learning.

#### 1.1. Unsupervised Machine Learning 
To identify the area of interests (respectiveley the "predefined words" to be used in Collocation Analysis), we run an unsupervised machine learning algorithm and analyse the resulting patterns. 

#### 1.2. Supervised Machine Learning
If the resulsts from unsupervised machine learning is promising/sensible, we can go for a supervised machine learning as well.

Along with the outcome of the unsupervised learning, we can also use the words obtained by Simple Freqeuncy Analysis (see item 2 below). 

### 2. Simple Frequency Analysis
Word frequency analysis in the program descriptions can be done. This will be a simple (and cost effective) way to extract the following insights:
* Mostly mentioned technologies in cyber security, cloud technologies, 
* Bugcrowd’s program (or business customer) profile. E.g. "which type of industries require bugcrowd services the most?"

### 3. Manual Scan
As a last resort, assuming items 1 and 2 have not produced fruitful results, we can manually scan program details focusing on program descriptions, metadata, company information etc. to identify similarities across programs.

### 4. What should **not** be done
* Sentiment Analysis won’t make sense since most probably all will turn out to be neutral.
* Lexical Diversity Analysis won't make sense since we are not interested in lingustic complexity of authors.
