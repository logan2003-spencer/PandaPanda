# This code takes the data frame from the "practice_names.csv" sheet and prints certain information from it such as the cities, names, and ages. It changes the salary and age of two of the individuals on the sheet and adds a new column to the sheet called "seniority. It then filters out the majority of the data and prints the information along with the mean salaries of the individuals from the same cities."

# Panda library is imported and referred to as pd.
import pandas as pd

# The sheet file is referenced and saved as myDF.
myDF = pd.read_csv("practice_names.csv")

# All the cities are listed in order that they are entered into on the sheet.
print(myDF["city"])

# Space between print statements.
print("\n")

# All the names and ages are printed.
print(myDF[["name","age"]])

# Space between print statements.
print("\n")

# The salary for the individual on the 9th row is changed to 56000 and all his information is printed.
myDF.at[8,"salary"] = 56000
print( myDF.iloc[8])

# Space between print statements.
print("\n")

# The age for the individual on the 2nd row is changed to be 29 years old and all her information is printed.
myDF.at[1,"age"] = 29
print(myDF.iloc[1])

# A new column called "seniority" is made and is moved to be the 4th column on the sheet.
myDF["seniority"] = [None] * len(myDF)
col_to_move = "seniority"
col = myDF[col_to_move]
myDF.drop(columns = [col_to_move], inplace = True)
myDF.insert(3, "seniority", col)

# The new column makes it so every individual over the age of 40 will be referenced as "experienced" in this specific column.
myDF.loc[myDF["age"]>40, "seniority"] = "experienced"

# The age column is all converted to be integers and the information is filtered to output all indiviuduals who are older than 35 and live in one of these three cities.
myDF["age"] = pd.to_numeric(myDF["age"])
myDF.query("(age > 35 and city == 'Boston') or (age > 35 and city == 'Seattle' or (age > 35 and city == 'San Francisco'))", inplace=True)

# Space between print statements.
print("\n")

# The data of the four individuals that were printed out is sorted by their salaries descending.
myDF.sort_values(["salary"], ascending= False, inplace = True)
print(myDF)

# The mean of these 4 individuals' salaries is calculated.
meanSalary = myDF["salary"].mean()

# Space between print statements.
print("\n")

# The mean of the salaries is printed
print(meanSalary)


# A new data set is made with the three same cities and salaries.
newDataFrame = {
    "City": ["Boston", "Boston", "San Francisco", "Seattle"],
    "Salary":[91000, 72000, 58000, 94000]
}

# The new data set is referenced to a variable.
newFrame = pd.DataFrame(newDataFrame)

# The mean salaries for each of the cities is calculated.
meanCitySalary = newFrame.groupby("City")["Salary"].mean()

# Space between print statements 
print("\n")

# The mean salaries for each of the cities is printed.
print(meanCitySalary)

# The variable, meanCitySalary and the information referenced with it is saved to a new spreadsheet and the information is printed on the sheet.
meanCitySalary.to_excel("mean_salary_by_city.xlsx", index=False)
