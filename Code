import pandas as pd
import matplotlib.pyplot as plt
from fpdf import FPDF
import sys

# Step 1: Define file names
report_file = "disaster_report.txt"
chart_file = "disaster_trend.png"
pdf_file = "disaster_report.pdf"

# Step 2: Load dataset
try:
    df = pd.read_csv("disasters.csv", parse_dates=["Date"])
except FileNotFoundError:
    print("Error: 'disasters.csv' not found.")
    sys.exit()

# Step 3: Redirect output to a text file
with open(report_file, "w") as f:
    sys.stdout = f  # Redirect print output

    print("=== Dataset Overview ===")
    print(df.head())

    print("\n=== Disaster Counts ===")
    print(df['DisasterType'].value_counts())

    print("\n=== Disasters by Location ===")
    location_disasters = df['Location'].value_counts()
    print(location_disasters)

    print("\n=== Average Fatalities and Injuries by Disaster Type ===")
    severity_means = df.groupby('DisasterType')[['Fatalities', 'Injuries']].mean()
    print(severity_means)

    print("\n=== Yearly Trend ===")
    df['Year'] = df['Date'].dt.year
    yearly_disasters = df.groupby('Year')['DisasterType'].count().sort_index()
    print(yearly_disasters)

    print("\n=== Predicted Most Likely Next Disaster ===")
    if not df['DisasterType'].empty:
        predicted_disaster = df['DisasterType'].mode()[0]
        print(predicted_disaster)
    else:
        print("No data available to predict the next disaster.")

    sys.stdout = sys.__stdout__  # Restore normal output

# Step 4: Plot and save the chart
plt.figure(figsize=(10, 6))
yearly_disasters.plot(kind='bar', color='skyblue', edgecolor='black')
plt.title("Number of Disasters Per Year")
plt.xlabel("Year")
plt.ylabel("Number of Disasters")
plt.xticks(rotation=45)
plt.grid(axis='y', linestyle='--', alpha=0.7)
plt.tight_layout()
plt.savefig(chart_file)
plt.close()

# Step 5: Create PDF report
pdf = FPDF()
pdf.add_page()
pdf.set_font("Arial", size=12)

# Add text from file
with open(report_file, "r") as f:
    for line in f:
        pdf.multi_cell(0, 10, line)

# Add image chart
pdf.add_page()
pdf.image(chart_file, x=10, y=20, w=180)

pdf.output(pdf_file)
print(f"PDF report generated: {pdf_file}")
