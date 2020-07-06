
Pybank Menacho
image

Your task is to create a Python script that analyzes the records to calculate each of the following:

The total number of months included in the dataset.

The net total amount of Profit/Losses over the entire period.

The average of the changes in Profit/Losses over the entire period.

The greatest increase in profits (date and amount) over the entire period.

The greatest decrease in losses (date and amount) over the entire period.

Your resulting analysis should look similar to the following:

Financial Analysis
Total Months: 86 Total: $38382578
Average  Change: $-2315.12 Greatest Increase in Profits: Feb-2012 ($1926159)
Greatest Decrease in Profits: Sep-2013 ($-2196167)

In [55]:
# Imports
import csv
from pathlib import Path

budget_data = Path("budget_data.csv")
budget_output = Path("budget_analysis.txt")

# Notes: index column 0 = date and index column 1 = profit/loss
# Track various financial parameters
total_months = 0
month_of_change = []
net_change_list = []
greatest_increase = ["", 0]
greatest_decrease = ["", 9999999999]
total_net = 0

# Read the csv and convert it into a list of dictionaries
with open(budget_data) as financial_data:
    reader = csv.reader(financial_data)

    # Read the header row
    header = next(reader)

    # Extract first row to avoid appending to net_change_list
    first_row = next(reader) # header and first_row are the same but for calculation purposes
    total_months = total_months + 1
    total_net = total_net + int(first_row[1])
    prev_net = int(first_row[1])

    for row in reader:

        # Track the total
        total_months = total_months + 1
        total_net = total_net + int(row[1])
        # Track the net change
        net_change = int(row[1]) - prev_net
        prev_net = int(row[1])
        net_change_list = net_change_list + [net_change]
        month_of_change = month_of_change + [row[0]]

        # Calculate the greatest increase
        if net_change > greatest_increase[1]:
            greatest_increase[0] = row[0]
            greatest_increase[1] = net_change

        # Calculate the greatest decrease
        if net_change < greatest_decrease[1]:
            greatest_decrease[0] = row[0]
            greatest_decrease[1] = net_change

# Calculate the Average Net Change
net_monthly_avg = round(sum(net_change_list) / len(net_change_list),2)
print(f"Financial Analysis")
print(f"-------------------\n")
print(f"Total Months: {total_months}\n")
print(f"Total Net Profits: ${total_net}\n")
print(f"Average Change: ${net_monthly_avg}\n")
print(f"Greatest Increase in Profits: {greatest_increase[0]} (${greatest_increase[1]})\n")
print(f"Greatest Decreease in Profits: {greatest_decrease[0]} (${greatest_decrease[1]})\n")

# Export the results to text file
with open(budget_output, "w") as txt_file:
    # with open 'w' = write with open 'r' = read
    txt_file.write(f"Financial Analysis\n")
    txt_file.write(f"----------------------------\n")
    txt_file.write(f"Total Months: {total_months}\n")
    txt_file.write(f"Total: ${total_net}\n")
    txt_file.write(f"Average  Change: ${net_monthly_avg}\n")
    txt_file.write(f"Greatest Increase in Profits: {greatest_increase[0]} (${greatest_increase[1]})\n")
    txt_file.write(f"Greatest Decrease in Profits: {greatest_decrease[0]} (${greatest_decrease[1]})\n")
Financial Analysis
-------------------

Total Months: 86

Total Net Profits: $38382578

Average Change: $-2315.12

Greatest Increase in Profits: Feb-12 ($1926159)

Greatest Decreease in Profits: Sep-13 ($-2196167)
