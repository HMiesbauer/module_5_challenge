Module 5 Challenge Assignment Instructions here
Compare the performance of Pymaceuticals’ drug of interest, Capomulin, against the other treatment regimens.
Generate all of the tables and figures needed for the technical report of the clinical study. Write at least three observations or inferences that can be made from the data.

Code sources

# Use groupby and summary statistical methods to calculate the following properties of each drug regimen: 
# mean, median, variance, standard deviation, and SEM of the tumor volume. (ChatGPT, personal communication, January 19, 2023)

sem_tumor = drop_duplicate_mice.groupby('Drug Regimen')['Tumor Volume (mm3)'].sem()
variance_tumor = drop_duplicate_mice.groupby('Drug Regimen')['Tumor Volume (mm3)'].var()
std_deviation_tumor = drop_duplicate_mice.groupby('Drug Regimen')['Tumor Volume (mm3)'].std()

statistics_table['Tumor Volume Std. Error'] = sem_tumor
statistics_table['Tumor Volume Variance'] = variance_tumor
statistics_table['Tumor Volumen Std. Dev'] = std_deviation_tumor


##Quartiles, Outliers and Boxplots
chatgpt 
# Calculate the final tumor volume of each mouse across four of the treatment regimens: (ChatGPT, personal communication, January 23, 2024)

# Capomulin, Ramicane, Infubinol, and Ceftamin

drugs = ['Capomulin', 'Ramicane', 'Infubinol', 'Ceftamin']
tumor_vol = drop_duplicate_mice[drop_duplicate_mice['Drug Regimen'].isin(drugs)]
final_volume = tumor_vol.groupby('Mouse ID')['Tumor Volume (mm3)'].last()

final_volume

# Start by getting the last (greatest) timepoint for each mouse

current_timepoint = tumor_vol.groupby('Mouse ID')['Timepoint'].idxmax()
final_tumor_data = tumor_vol.loc[current_timepoint, ['Mouse ID', 'Tumor Volume (mm3)']]
final_tumor_data


# Calculate the final tumor volume of each mouse across four of the treatment regimens:  (ChatGPT, personal communication, January 23, 2024)
# Capomulin, Ramicane, Infubinol, and Ceftamin

drugs = ['Capomulin', 'Ramicane', 'Infubinol', 'Ceftamin']
tumor_vol = drop_duplicate_mice[drop_duplicate_mice['Drug Regimen'].isin(drugs)]
final_volume = tumor_vol.groupby('Mouse ID')['Tumor Volume (mm3)'].last()

final_volume

# Start by getting the last (greatest) timepoint for each mouse (ChatGPT, personal communication, January 23, 2024)

current_timepoint = tumor_vol.groupby('Mouse ID')['Timepoint'].idxmax()
final_tumor_data = tumor_vol.loc[current_timepoint, ['Mouse ID', 'Tumor Volume (mm3)']]
final_tumor_data

# Merge this group df with the original DataFrame to get the tumor volume at the last timepoint
final_tumor_data = pd.merge(drop_duplicate_mice, final_volume, on='Mouse ID', how='inner')
final_tumor_data.head(25)

# Calculate the IQR and quantitatively determine if there are any potential outliers. (samples_solution in class exercise)

quartiles = treatment_data.quantile([0.25, 0.5, 0.75])
lower_quartile = quartiles[0.25]
upper_quartile = quartiles[0.75]
iqr = upper_quartile - lower_quartile

    
# Locate the rows which contain mice on each drug and get the tumor volumes # Calculate the final tumor volume of each mouse across four of the treatment regimens:  (ChatGPT, personal communication, January 23, 2024)
# Capomulin, Ramicane, Infubinol, and Ceftamin

drugs = ['Capomulin', 'Ramicane', 'Infubinol', 'Ceftamin']
tumor_vol = drop_duplicate_mice[drop_duplicate_mice['Drug Regimen'].isin(drugs)]
final_volume = tumor_vol.groupby('Mouse ID')['Tumor Volume (mm3)'].last()

final_volume

# Start by getting the last (greatest) timepoint for each mouse (ChatGPT, personal communication, January 23, 2024)

current_timepoint = tumor_vol.groupby('Mouse ID')['Timepoint'].idxmax()
final_tumor_data = tumor_vol.loc[current_timepoint, ['Mouse ID', 'Tumor Volume (mm3)']]
final_tumor_data

# Merge this group df with the original DataFrame to get the tumor volume at the last timepoint
final_tumor_data = pd.merge(drop_duplicate_mice, final_volume, on='Mouse ID', how='inner')
final_tumor_data.head(25)tumor_drugtreatment_volume = {}
for treatment in treatments:
    treatment_data = final_tumor_data[final_tumor_data["Drug Regimen"] == treatment]
    tumor_volumes = treatment_data["Tumor Volume (mm3)_x"]
    tumor_drugtreatment_volume[treatment] = tumor_volumes
print(tumor_drugtreatment_volume)  
    
# Determine outliers using upper and lower bounds. (samples_solution in class exercise)

lower_bound = lower_quartile - 1.5 * iqr
upper_bound = upper_quartile + 1.5 * iqr


# Create a list of tumor volumes for each treatment (ChatGPT, personal communication, January 23, 2023)
tumor_volume_lists = [treatment_data_dict[treatment]['All Data'] for treatment in treatments]


# Generate a box plot [https://matplotlib.org/3.3.3/gallery/pyplots/boxplot_demo_pyplot.html#sphx-glr-gallery-pyplots-boxplot-demo-pyplot-py](https://matplotlib.org/gallery/pyplots/boxplot_demo_pyplot.html#sphx-glr-gallery-pyplots-boxplot-demo-pyplot-py)
flierprops=dict(markerfacecolor='r', marker='o'))

# Plot the different factors in a scatter plot (sourced from regression_solution in class exercise)
# Generate a scatter plot of mouse weight vs. the average observed tumor volume for the entire Capomulin regimen. 
#Code sourced (ChatGPT, personal communication, January 23, 2024)

# Choose the Capomulin regimen (ChatGPT, personal communication, January 23, 2024)
capomulin_data = final_tumor_data[final_tumor_data['Drug Regimen'] == 'Capomulin']

# Calculate the average observed tumor volume for each mouse. (ChatGPT, personal communication, January 23, 2024)
average_tumor_volume = capomulin_data.groupby('Mouse ID')['Tumor Volume (mm3)_y'].mean().reset_index()

# Merge the average tumor volume data with the weight data. (ChatGPT, personal communication, January 23, 2024)
merged_data = pd.merge(capomulin_data[['Mouse ID', 'Weight (g)']].drop_duplicates(), average_tumor_volume, on='Mouse ID')

# Create a scatter plot. (ChatGPT, personal communication, January 23, 2024)
plt.scatter(merged_data['Weight (g)'], merged_data['Tumor Volume (mm3)_y'])
plt.xlabel('Weight (g)')
plt.ylabel('Average Tumor Volume (mm3)')
plt.title('Mouse Weight vs. Average Tumor Volume (Capomulin Regimen)')


# Calculate the correlation coefficient and a linear regression model 
# for mouse weight and average observed tumor volume for the entire Capomulin regimen
#Code sourced (ChatGPT, personal communication, January 23, 2024)

# Choose the Capomulin regimen. (ChatGPT, personal communication, January 23, 2024)
capomulin_data = final_tumor_data[final_tumor_data['Drug Regimen'] == 'Capomulin']

# Calculate the average observed tumor volume for each mouse. (ChatGPT, personal communication, January 23, 2024)
average_tumor_volume = capomulin_data.groupby('Mouse ID')['Tumor Volume (mm3)_y'].mean().reset_index()

# Merge the average tumor volume data with the weight data. (ChatGPT, personal communication, January 23, 2024)
merged_data = pd.merge(capomulin_data[['Mouse ID', 'Weight (g)']].drop_duplicates(), average_tumor_volume, on='Mouse ID')

# Calculate the correlation coefficient. (ChatGPT, personal communication, January 23, 2024)
correlation_coefficient, _ = st.pearsonr(merged_data['Weight (g)'], merged_data['Tumor Volume (mm3)_y'])
print(f"Correlation Coefficient: {correlation_coefficient}")

# Perform linear regression (ChatGPT, personal communication, January 23, 2024)
X = merged_data[['Weight (g)']]
y = merged_data['Tumor Volume (mm3)_y']

# Create and fit the linear regression model. (ChatGPT, personal communication, January 23, 2024)
model = LinearRegression()
model.fit(X, y)

# Predict tumor volume using the linear regression model. (ChatGPT, personal communication, January 23, 2024)
predictions = model.predict(X)

# Plot the linear regression line. (ChatGPT, personal communication, January 23, 2024)
plt.scatter(X, y, label='Observations')
plt.plot(X, predictions, color='red', label='Linear Regression')
plt.xlabel('Weight (g)')
plt.ylabel('Average Tumor Volume (mm3)')
plt.title('Mouse Weight vs. Average Tumor Volume (Capomulin Regimen)')
plt.legend()
plt.show()
