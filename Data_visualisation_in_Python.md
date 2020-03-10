###### tags: `python` `plotly.express.scatter()` `plotly.io.write_html()`

# Data visualisation in Python

Chang’s working examples created in Python

---

## Colours

### Pick a RGB color code from rgb color picker

![RGB Colour picker](https://i.imgur.com/hVbdjBO.png)

---

## Packages

### plotly-orca
`conda install -c plotly plotly-orca psutil requests` install this package via conda. pip.exe is not working
[Static Image Export in Python](https://plot.ly/python/static-image-export/)
[Why `plotly-orca` is unable to install via `pip`](https://github.com/plotly/orca/issues/194)

![conda install -c plotly plotly-orca psutil requests](https://i.imgur.com/4EZJRyl.png)

---

## Plot types

### Box plot

:::info
Create one box plot[^1], visualising 3 continuous variables that are not normally distributed- Heart rate, respiratory rate, and system providing oxygen saturation (SpO2)
:::

[^1]: **Python script file path:**
D:\googleDrive\Job\UQ_509098_Data-analyst_Paediatric-Critical-Care-Research-Group\Practical-Assessment\Chang_PCCRG_dashboard_visualise-HeartRate-RespiratoryRate-spo2.ipynb

**plot file path:** 
D:\googleDrive\Job\UQ_509098_Data-analyst_Paediatric-Critical-Care-Research-Group\Practical-Assessment\boxplot_HeartRate-RespiratoryRate-spo2_full-sample.png

![boxplot](https://i.imgur.com/9lHVYKl.png)

---

:::info
Create one box plot with 2 facet columns[^2], visualising the 3 continuous variables that are not normally distributed-- Heart rate, respiratory rate, and system providing oxygen saturation (SpO2)-- in different study hospitals
:::

[^2]: **Python script file path:**
D:\googleDrive\Job\UQ_509098_Data-analyst_Paediatric-Critical-Care-Research-Group\Practical-Assessment\Chang_PCCRG_dashboard_visualise-HeartRate-RespiratoryRate-spo2.ipynb

**plot file path:** 
D:\googleDrive\Job\UQ_509098_Data-analyst_Paediatric-Critical-Care-Research-Group\Practical-Assessment\boxplot_HeartRate-RespiratoryRate-spo2_in-3-study-sites.png

![boxplot-facet_col](https://i.imgur.com/XCDaTlM.png)

---

## Save plots

### Save 1 plotly plot as a html
[How To Create a Plotly Visualization And Embed It On Websites](https://towardsdatascience.com/how-to-create-a-plotly-visualization-and-embed-it-on-websites-517c1a78568b)

```python!
import plotly.express as px
# Create a scatter plot by plotly
fig = px.scatter(df, x='Neighborhood', y='Price (US Dollars)'
                 ,size='Accommodates'
                 , hover_data=['Bedrooms', 'Wifi', 'Cable TV', 'Kitchen', 'Washer', 'Number of Reviews']
                 ,color= 'Room Type')
fig.update_layout(template='plotly_white')
fig.update_layout(title='How much should you charge in a Berlin neighborhood?')
fig.show()

# To generate the HTML file for the plotly visualization, use:
import plotly.io as pio
pio.write_html(fig, file=’index.html’, auto_open=True)
```

---

### Save multiple plotly plots as a html
[Plotly saving multiple plots into a single html (python)](https://stackoverflow.com/questions/59868987/plotly-saving-multiple-plots-into-a-single-html-python)

```python!
import plotly.express as px
# Make plot 1
# Box plots for full sample
fig1= px.box(data3, title="Box plot of heart rate, respiratory rate and SpO2", x="variable", y="value",notched=True, points="all",color="Study Hospital", template='presentation') 
# Tweaking the default setting in plotly objects
fig1.update_xaxes(tickangle=0, tickfont=dict(family='Rockwell', color='crimson', size=15))

# Make plot 2
# Box plots in study groups
fig2= px.box(data3, title="Box plot of heart rate, respiratory rate and SpO2", x="variable", y="value",notched=True, points="all",color="Study Hospital", facet_col='Study Group', template='presentation') # , template='presentation'
# Tweaking the default setting in plotly objects
fig2.update_xaxes(tickangle=0, tickfont=dict(family='Rockwell', color='crimson', size=15))

# Save the 2 plotly plots as a HTML file
## full_html=False which will give you just DIV containing figure. You can just write multiple figures to one HTML by appending DIVs containing figures
output_file_path= "D:/googleDrive/Job/UQ_509098_Data-analyst_Paediatric-Critical-Care-Research-Group/Practical-Assessment/boxplots.html"
with open(output_file_path, 'a') as f:
    f.write(fig1.to_html(full_html=False, include_plotlyjs='cdn'))
    f.write(fig2.to_html(full_html=False, include_plotlyjs='cdn'))
```
