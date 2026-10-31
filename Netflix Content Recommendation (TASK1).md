**Netflix Content Recommendation System: Task 1 Documentation**



Auspify Technologies Machine Learning Internship | Task 1 (Easy) | Author: Elliot Agyare





1\. **Project Overview**



**Objective:** build a machine learning based recommendation system that suggests similar Netflix titles based on genres, categories and other content attributes.





**Approach** : each title is described by a short line of text (its genres, director, country, type and audience rating). That text is converted to numbers with TF-IDF, and the similarity between two titles is measured with cosine similarity. The system recommends the titles with the highest similarity to a title the viewer likes. i used content-based approach because that's how i actually  search for movies.


**2. Dataset**



• **File**: Dataset.csv, 8,790 rows and 10 columns.


• **Columns**: show\_id, type, title, director, country, date\_added, release\_year, rating, duration, listed\_in.


• **Type split**: 6,126 Movies and 2,664 TV Shows.


• **Data quality**: missing values are stored as the text "Not Given" (2,588 directors and 287 countries) rather than as real blanks. There are also 3 duplicate rows.


• **Limitation**: there is no plot description column, so similarity relies on metadata only.


**3. Tools and Libraries**

• Python in Google Colab


• pandas and numpy for data handling



• scikit-learn: TfidfVectorizer for text-to-number conversion and cosine\_similarity for scoring


**4. Methodology**

Step 1: Prepare content-related features

• Replaced the "Not Given" placeholders with empty text and removed duplicate rows, leaving 8,787 titles.


• Wrote a clean\_list() function that splits comma-separated values and joins multi-word names with underscores (for example "TV Dramas" becomes tv\_dramas), so each category stays a single word.


• Built text features from genres (listed\_in), director, country, type and rating.

• Combined them into one text column called "soup".






Step 2: Convert text data into machine-readable format

• Used TfidfVectorizer to turn each soup into a row of numbers. Rare words, such as a specific director, receive more weight than common ones.


• Set token\_pattern to \\S+ so every space-separated word is one token. The default tokenizer would split ratings like tv-ma into two pieces and drop single-letter ratings such as R and G.




Step 3: Calculate content similarity scores

• Used cosine similarity, which compares the direction of two titles' number rows. A score of 1 means identical features and 0 means nothing in common.



Step 4: Generate recommendations for selected titles


• The recommend() function finds the chosen title, scores it against every other title, removes the title itself, and returns the top results with their similarity scores.



Step 5: Evaluate recommendation quality


• Took a random sample of titles and checked how often each recommendation shares at least one genre with the original title.


• Compared this with a random baseline (random titles instead of recommended ones).


• Caveat: genres are part of the model's features, so this check is partly circular. It shows the system works better than random, but it is not a measure of real viewer satisfaction.


**5. Results**


Example recommendations

|Title searched|Top  recommendation|Do they make sense?|
|-|-|-|
|Narcos |Queen of the South	|yes|
|Marvel's Luke Cage	|Marvel's The Defenders|yes|
|Gotham	|Altered Carbon|no|




Effect of genre weight

("Narcos") with GENRE\_WEIGHT = 1 . gives you shows closely related and with GENRE\_WEIGHT = 3, adds random and diverse shows 



**Quality check**



• Recommendations sharing a genre: 1

• Random picks sharing a genre: 3

• if you want to stay on track the best option will be 1 



**6. Challenges and How They Were Solved**


• Words glued together: a missing space when joining text columns merged two words into one fake word. Fixed by adding a space between every part of the soup.


• Ratings lost by the tokenizer: the default tokenizer split tv-ma and dropped single-letter tokens. Fixed with token\_pattern = \\S+.


• Changes not showing: after changing the features, the old matrix was still in memory. Fixed by re-running the TF-IDF and recommendation cells after every change.


• when the section is expired the code doesn't run well


7\. Limitations


• No plot descriptions or cast, so titles with identical metadata get identical scores.

• The evaluation uses a proxy measure, not real watch history or ratings.

• A content-based system recommends more of the same and rarely surprises the viewer.



8\. How to Run the Project


1\. Open Google Colab and create a new notebook.

2\. Upload Dataset.csv using the folder panel on the left.

3\. Run the cells from top to bottom with Shift + Enter.

4\. Change the title inside recommend("...") to get recommendations for any title in the dataset.



9\. Future Improvements


• Add plot descriptions and cast to the features if a fuller dataset is available.

• Tune the feature weights against real viewing data.

• Combine with collaborative filtering that uses viewer behavior.











&#x20;

