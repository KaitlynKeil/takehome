# Analyzer for Book Descriptions

## Written for the BookBub takehome challenge

Kaitlyn Keil

Updated: 10/13/2017

===============================================================

### Running the Analyzer

To run the analyzer from the terminal, call 'python3 book_desc_analyzer.py < json_file_name > < csv_file_name >'. The books contained within the json file will be analyzed according to the keywords and points within the csv, with the top three genres printed beneath the title.

The json file must be formatted as per the example, sample_book_json.json. The csv file must be formatted as per the example csv, sample_genre_keyword_value.csv, with a header row, genres in the first column, keywords in the second, and points in the third.

Dependencies: This program uses the json, csv, and sys libraries for python3.

### Choices Made

I chose to break this program into multiple function steps as opposed to one long program, despite the linear nature. This is largely because I find it easier to debug discrete sections and then put them together once I am certain they are working the way I expect. I also find it easier to read programs when they are formatted into small pieces.

At first, I had the csv being read in to a csv.DictReader. However, when trying to access the Points column, I realized there was a space: ' Points'. This made me switch to a csv.reader and access columns instead. While this makes it slightly less readable on first glance, it means that a different csv that might not have included those spaces could still be used in this program, assuming the order was correct.

I also chose to parse the description string as a whole, instead of splitting it into a list of words, mostly because some of the keywords were getting broken apart by splitting around spaces. Even though in these specific cases it didn't matter, I decided to replace punctuation with spaces, in case of strange formatting that might make the strings match incorrectly. This is a matter of two lines, which, if further testing found it to be harmful rather than helpful, could easily be removed.

Currently, if a keyword appears in two different genres (such as 'distant future' for both action and sci-fi), it recieves two dictionary entries in the keyword_dictionary, and is searched for twice in the description. This second inefficiency could be solved with having a list of 'searched for' words; however, this could cause significant delay as the list of words grew longer. This might be worth running timing tests against.

Another significant choice was using tuples of (keyword, genre) to map to the point values read from the csv. I also considered having a dictionary of dictionaries, where genres were used as keys into dictionaries of keywords and values, but ultimately decided that this was more confusing to write, iterate through, and read. So instead, I used the dictionaries somewhat like an array, with keyword as one axis and genre as another. This 'arary' structure came from realizing that 'distant future' as a keyword pointing to the tuple (genre, points) was being overwritten and losing information.

Inside score calculator, genre_dict (I have a fondness for dictionaries as a very useful data structure) is a dictionary that points to a tuple of three values: the sum of keyword-points-per-word, the count of distinct keywords for each genre that appeared inside the description, and then the total keywords for that genre that appeared. I dislike this section because I find it difficult to phrase the difference between the last two values, though it makes it very simple to find the average points per word and total score for each genre. I think that it is an easier calculation than trying to keep track of a running average, but would like to phrase it better.

#### Time taken:

2 hours for programming, 0.5 hours for write-up.

### File Directory

book_desc_analyzer.py: Python file that reads a json file and a csv file, then prints the names of the books within the json file, followed by the top three genres it fits, based on the description and keywords.

sample_genre_keyword_value.csv: csv file containing the desired format for keywords.

sample_book_json.json: json file containing the desired format for books and their descriptions.

bookbub_analyzer.js: testing script, run with node