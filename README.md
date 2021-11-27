# Election_Analysis


# Python_Election_Analysis
Purpose of this challenge is to analyze election results using python to read in a csv file and provide the following deliverables:

1. The Election Results Printed to the Command line
2. The Election Results Saved to a Text File
3. A written Analysis of the Election Audit (ReadMe.md)

## Overview of the Election Audit:

The Colorado Board of Elections have requested an Election Audit using the data in the election_results.csv file which consists of a number for the ballot ID ,a name for the county and candidate.

Our final Python Script will deliver the following information requested:
> * Total number of votes cast
> * A complete list of candidates who received votes
> * Total number of votes each candidate received
> * Percentage of votes each candidate won
> * The winner of the election based on popular vote

The election commission also asked for some additional data to complete the audit:
>* The voter turnout for each county
>* The percentage of votes from each county out of the total count
>* The county with the highest turnout


The results are expected to confirm that the output to the terminal matches the following template:

![my analysis](./Images/data-Module-3-Challenge-election-results-1.png)



# Resources
* Data source : election_results.csv
* Software : Python 3.7.6, Visual Code 1.61.2
* Additional resources : https://www.python.org/

## Election_Audit Results:
The analysis of the election show that:
- There were a total of 369,711 cast in this election.
- The candidates were:
    * Charles Casper Stockham
    * Diana DeGette
    * Raymon Anthony Doane
- The candidate results were:
    * Charles Casper Stockham: 23.0% (85,213)
    * Diana DeGette: 73.8% (272,892)
    * Raymon Anthony Doane: 3.1% (11,606)
- The winner of the election was:
    Diana DeGette: 73.8% (272,892)

### Specified "Asks":
>* How many votes were cast in this congressional election?
    - Total_Votes : 369,711
>* Provide a breakdown of the number of votes and the percentage of total votes for each county in the precinct.
    County Votes:
        - Jefferson: 10.5% (38,855)
        - Denver: 82.8% (306,055)
        - Arapahoe: 6.7% (24,801)
>* Which county had the largest number of votes?
    Denver: 82.8% (306,055)
>* Provide a breakdown of the number of votes and the percentage of the total votes each candidate received.
        - Charles Casper Stockham: 23.0% (85,213)
        - Diana DeGette: 73.8% (272,892)
        - Raymon Anthony Doane: 3.1% (11,606)
>* Which candidate won the election, what was their vote count, and what was their percentage of the total votes?
        Winner: Diana DeGette
        Winning Vote Count: 272,892
        Winning Percentage: 73.8%

## Results as recorded in my election_analysis.txt file:

![my analysis](./Images/Screen_Shot_2021-11-27.png)

## Python Code:

        # Add our dependencies.
        import csv
        import os

        # Add a variable to load a file from a path.
        file_to_load = os.path.join(".", "Resources", "election_results.csv")
        # Add a variable to save the file to a path.
        file_to_save = os.path.join(".","Analysis", "election_analysis.txt")

        # Initialize a total vote counter.
        total_votes = 0

        # Candidate Options and candidate votes.
        candidate_options = [] # initialize a list
        candidate_votes = {}  # initialize a dictionary

        # 1: Create a county list and county votes dictionary.
        county_names = []
        county_votes = {}

        # Track the winning candidate, vote count and percentage
        winning_candidate = ""
        winning_count = 0
        winning_percentage = 0

        # 2: Track the largest county and county voter turnout.
        largest_county_turnout = ""
        largest_county_vote = 0

        # Read the csv and convert it into a list of dictionaries
        with open(file_to_load) as election_data:
        reader = csv.reader(election_data)

    # Read the header
    header = next(reader)

    # For each row in the CSV file( starting on the second row).
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1

        # Get the candidate name from each row.
        candidate_name = row[2]

        # 3: Extract the county name from each row.
        county_name = row[1]

        # If the candidate does not match any existing candidate add it to
        # the candidate list
        if candidate_name not in candidate_options:

            # Add the candidate name to the candidate list.
            candidate_options.append(candidate_name)

            # And begin tracking that candidate's voter count.
            candidate_votes[candidate_name] = 0

        # Add a vote to that candidate's count
        candidate_votes[candidate_name] += 1

        # 4a: Write an if statement that checks that the
        # county does not match any existing county in the county list.
        if county_name not in county_names:

            # 4b: Add the existing county to the list of counties.
            county_names.append(county_name)
            
            # 4c: Begin tracking the county's vote count.
            county_votes[county_name]= 0


         # 5: Add a vote to that county's vote count.
        county_votes[county_name] += 1

    
    # I have iterated over all rows in the cvs file (all votes)

    # Save the results to our text file.
with open(file_to_save, "w") as txt_file:

    # Print the final vote count (to terminal)
    election_results = (
        f"\nElection Results\n"
        f"-------------------------\n"
        f"Total Votes: {total_votes:,}\n"
        f"-------------------------\n\n"
        f"County Votes:\n")
    print(election_results, end="")

    txt_file.write(election_results)

    # 6a: Write a for loop to get the county from the county dictionary.
    # print(county_votes)
    for county in county_votes:


        # 6b: Retrieve the county vote count.
        county_vote = county_votes[county]
        # 6c: Calculate the percentage of votes for the county.
        # county_percentage = float(county_vote)/ float(total_votes) *100
        county_percentage = float(county_vote) / float(total_votes) * 100


        # 6d: Print the county results to the terminal.
        # create an f-string
        county_results = (
             f"{county}: {county_percentage:.1f}% ({county_vote:,})\n"
         )
        
        print(county_results) # print the f string to the terminal

    

        # 6e: Save the county votes to a text file.
        # write the f-string to my text file
        txt_file.write(county_results)

        # 6f: Write an if statement to determine the winning county and get its vote count.
        if (county_vote > largest_county_vote): 
            largest_county_vote = county_vote
            largest_county_turnout = county
    # now we have iterated over all counties

    # 7: Print the county with the largest turnout to the terminal.
    largest_county_turnout = (
        f"\n-------------------------\n"
        f"Largest County Turnout: {largest_county_turnout}\n"
        f"-------------------------\n"
    )
    print(largest_county_turnout)


    # 8: Save the county with the largest turnout to a text file.
    txt_file.write(largest_county_turnout)

    # Save the final candidate vote count to the text file.
    for candidate_name in candidate_votes:

        # Retrieve vote count and percentage
        votes = candidate_votes.get(candidate_name)
        vote_percentage = float(votes) / float(total_votes) * 100
        candidate_results = (
            f"{candidate_name}: {vote_percentage:.1f}% ({votes:,})\n")

        # Print each candidate's voter count and percentage to the
        # terminal.
        print(candidate_results)
        #  Save the candidate results to our text file.
        txt_file.write(candidate_results)

        # Determine winning vote count, winning percentage, and candidate.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage

    # Print the winning candidate (to terminal)
    winning_candidate_summary = (
        f"-------------------------\n"
        f"Winner: {winning_candidate}\n"
        f"Winning Vote Count: {winning_count:,}\n"
        f"Winning Percentage: {winning_percentage:.1f}%\n"
        f"-------------------------\n")
    print(winning_candidate_summary)

    # Save the winning candidate's name to the text file
    txt_file.write(winning_candidate_summary)

## Election_Audit Python Code Summary:
The python code provided can be used for any state wide election given the follow:

1. First, a separate csv file is neeeded for each election.
2. Second, a separate election analysis txt file will need to be created to record results.
3. Third, the candidates or counties are not "Hard-wired" to the code therefore it can be used for any state wide elections.
