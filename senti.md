
### Steps:

#### 1. **Install Required Libraries**
Ensure you have the following libraries installed:
```bash
pip install pandas textblob
```

#### 2. **Prepare Your Data**
Your `tweets.csv` file should have a structure similar to:
```csv
Tweet
"AI is amazing!"
"Machine learning is hard to learn."
"I love working on new technologies."
```

#### 3. **Python Code for Sentiment Analysis**
```python
import pandas as pd
from textblob import TextBlob

# Load tweets from CSV
file_path = "tweets.csv"  # Replace with your file path
df = pd.read_csv(file_path)

# Function to analyze sentiment
def analyze_sentiment(text):
    blob = TextBlob(text)
    return blob.sentiment.polarity, blob.sentiment.subjectivity

# Apply sentiment analysis to each tweet
df['Polarity'], df['Subjectivity'] = zip(*df['Tweet'].apply(analyze_sentiment))

# Categorize tweets based on polarity
def classify_sentiment(polarity):
    if polarity > 0:
        return "Positive"
    elif polarity < 0:
        return "Negative"
    else:
        return "Neutral"

df['Sentiment'] = df['Polarity'].apply(classify_sentiment)

# Save results to a new CSV file
output_file = "tweets_with_sentiments.csv"
df.to_csv(output_file, index=False)

# Display results
print(df)
```

#### 4. **Explanation of the Code**
1. **Load CSV**: Use `pandas` to load the tweets from `tweets.csv`.
2. **Analyze Sentiment**:
   - `Polarity`: Sentiment polarity ranging from -1 (negative) to 1 (positive).
   - `Subjectivity`: Measures how subjective (0 to 1) the tweet is.
3. **Categorize Sentiment**: Classify tweets as `Positive`, `Negative`, or `Neutral`.
4. **Save Results**: Save the output to a new CSV file.

#### 5. **Sample Output**
##### Input (`tweets.csv`):
```csv
Tweet
"AI is amazing!"
"Machine learning is hard to learn."
"I love working on new technologies."
```

##### Output (`tweets_with_sentiments.csv`):
```csv
Tweet,Polarity,Subjectivity,Sentiment
"AI is amazing!",0.8,0.75,Positive
"Machine learning is hard to learn.",-0.4,0.8,Negative
"I love working on new technologies.",0.5,0.6,Positive
```

#### 6. **Optional: Visualize Results**
You can plot the sentiments for better insights:
```python
import matplotlib.pyplot as plt

# Plot sentiment counts
df['Sentiment'].value_counts().plot(kind='bar', color=['green', 'red', 'blue'])
plt.title("Sentiment Analysis of Tweets")
plt.xlabel("Sentiment")
plt.ylabel("Number of Tweets")
plt.show()
```

This approach is efficient and avoids the need for the Twitter API while working directly with your dataset.
