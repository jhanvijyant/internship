import pandas as pd
from datetime import datetime, timedelta

# Sample data for demonstration purposes
data = {
    'Fund Name': ['Zerodha Midcap Fund', 'Zerodha Midcap Fund', 'HDFC Equity Fund', 'HDFC Equity Fund', 'Axis Bluechip Fund'],
    'Date': ['2023-01-01', '2023-02-01', '2023-01-01', '2023-02-01', '2023-01-01'],
    'Allocation': [100000, 120000, 150000, 140000, 200000]
}

# Create a DataFrame
df = pd.DataFrame(data)
df['Date'] = pd.to_datetime(df['Date'])

def load_data():
    """Load mutual fund data from a CSV file."""
    # In a real scenario, you would load data from a CSV or database
    return df

def get_fund_changes(fund_name, start_date, end_date):
    """Get changes in fund allocations for a specific fund within a date range."""
    # Load data
    data = load_data()
    
    # Filter data based on user input
    filtered_data = data[(data['Fund Name'] == fund_name) & 
                          (data['Date'] >= start_date) & 
                          (data['Date'] <= end_date)]
    
    # Check if there are any changes
    if filtered_data.empty:
        return f"No changes found for {fund_name} in the specified date range."
    
    # Group by month and fund name to see changes
    monthly_changes = filtered_data.groupby(filtered_data['Date'].dt.to_period('M')).sum().reset_index()
    monthly_changes['Date'] = monthly_changes['Date'].dt.to_timestamp()
    
    return monthly_changes

def main():
    # User input
    fund_name = input("Enter the Fund Name: ")
    months = int(input("Enter the number of months for the date range: "))
    
    # Calculate date range
    end_date = datetime.now()
    start_date = end_date - timedelta(days=months * 30)  # Approximate month length
    
    # Get fund changes
    changes = get_fund_changes(fund_name, start_date, end_date)
    
    # Display results
    if isinstance(changes, pd.DataFrame):
        print("\nChanges in Fund Allocations:")
        print(changes)
    else:
        print(changes)

if __name__ == "__main__":
    main()
