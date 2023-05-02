Download Link: https://assignmentchef.com/product/solved-comp1007-lab-8-staff-salary-calculation-and-charts
<br>
In this section, we are going to learn how to use advance feature of <strong>Pandas</strong> package to calculate staff salaries and use <strong>Matplotlib</strong> package to create different types of charts.

By finished this section, you will be able to:

<ul>

 <li>Retrieve the content of the CSV files and create data frames.</li>

 <li>Perform calculation with DataFrame.</li>

 <li>Create summaries using Pivot Table.</li>

 <li>Plot graphs.</li>

</ul>

<h1>Downloading the Data Files and Creating a Code File</h1>

Download the files <strong>door_access_records.csv</strong> and <strong>hourly_rates.csv</strong> from the BU eLearning web site and save them at your Spyder working directory.

<h1>Importing Required Library</h1>

Add the following lines to import the libraries:

import pandas as pd import numpy as np

<h1>Creating a Data Frame with the CSV File Content</h1>

Firstly, we have to load the data from the data file and create a data frame.

<ol>

 <li>Load the door access records from the file named <strong>csv</strong> and create a data frame named <strong>records</strong>. The <strong>pd.read_csv()</strong> function accepts a file name and returns a data frame.</li>

</ol>

Add the following line to create a data frame.

records = pd.read_csv(‘door_access_records.csv’)

The data is stored in the <strong>records</strong> data frame as follows:

<table width="0">

 <tbody>

  <tr>

   <td width="54"> </td>

   <td width="84"><strong>Date </strong></td>

   <td width="88"><strong>Staff Name </strong></td>

   <td width="134"><strong>Department </strong></td>

   <td width="192"><strong>Working Hours (In &amp; Out) </strong></td>

  </tr>

  <tr>

   <td width="54">0</td>

   <td width="84">6/15/2018</td>

   <td width="88">Jacky Lee</td>

   <td width="134">IT</td>

   <td width="192">6:12-20:44</td>

  </tr>

  <tr>

   <td width="54">1</td>

   <td width="84">6/15/2018</td>

   <td width="88">Joyce Ho</td>

   <td width="134">Human Resources</td>

   <td width="192">8:26-19:51</td>

  </tr>

  <tr>

   <td width="54">2</td>

   <td width="84">6/15/2018</td>

   <td width="88">Ivy Chu</td>

   <td width="134">Sales</td>

   <td width="192">8:29-18:44</td>

  </tr>

  <tr>

   <td width="54">3</td>

   <td width="84">6/15/2018</td>

   <td width="88">David Chan</td>

   <td width="134">Purchasing</td>

   <td width="192">8:33-19:50</td>

  </tr>

  <tr>

   <td width="54">4</td>

   <td width="84">6/15/2018</td>

   <td width="88">Gary Yuen</td>

   <td width="134">Logistics</td>

   <td width="192">8:34-19:45</td>

  </tr>

  <tr>

   <td width="54">…</td>

   <td width="84">…</td>

   <td width="88">…</td>

   <td width="134">…</td>

   <td width="192">…</td>

  </tr>

 </tbody>

</table>







<ol start="2">

 <li>Load the hourly salary rates from another file named <strong>csv</strong> and create a data frame named <strong>hourly_rates</strong>.</li>

</ol>




hourly_rate = pd.read_csv(‘hourly_rate.csv’, index_col = ‘Staff Name’)

With <strong>the index_col</strong> parameter, we set the column ‘Staff Name’ as an index. The data is stored in the <strong>hourly_rates</strong> data frame as follows:




<table width="0">

 <tbody>

  <tr>

   <td width="108"><strong> </strong></td>

   <td width="156"><strong>Salary Rate (per hour) </strong></td>

  </tr>

  <tr>

   <td width="108"><strong>Staff Name </strong></td>

   <td width="156"><strong> </strong></td>

  </tr>

  <tr>

   <td width="108">Jacky Lee</td>

   <td width="156">60</td>

  </tr>

  <tr>

   <td width="108">Joyce Ho</td>

   <td width="156">70</td>

  </tr>

  <tr>

   <td width="108">Ivy Chu</td>

   <td width="156">55</td>

  </tr>

  <tr>

   <td width="108">David Chan</td>

   <td width="156">65</td>

  </tr>

  <tr>

   <td width="108">Gary Yuen</td>

   <td width="156">75</td>

  </tr>

  <tr>

   <td width="108">…</td>

   <td width="156">…</td>

  </tr>

 </tbody>

</table>




<h1>Computing the Working Hours</h1>

In the records data frame, the “Working Hours (In &amp; Out)” column does not really store the working hours. It stores the IN time and OUT time of the staffs on individual days. The IN time and OUT time are stored in the same column with a minus sign as a separator. Now, we split the values and use them to compute the working hours. The following formula are used to compute the working hours:

(       )−           (                   )

=

60

<ol start="3">

 <li>Split the “Working Hours (In &amp; Out)” column by using the <strong>split()</strong> function. We temporarily store the values in two sets of Series – <strong>in_time</strong> and <strong>out_time</strong>.</li>

</ol>




in_time, out_time = records[‘Working Hours (In &amp; Out)’].str.split(‘-‘).str




The <strong>str.split()</strong> function accepts a string for splitting the values. In this example, we use a minus sign. And, we use <strong>.str</strong> to convert the results to string format. Now, <strong>in_time</strong> and <strong>out_time</strong> store time strings respectively.




<ol start="4">

 <li>Store the string versions of <strong>IN</strong> time and <strong>OUT</strong> time in the records data frame.</li>

</ol>

records[‘In’] = in_time

records[‘Out’] = out_time




The records data frame has two more columns now.




<ol start="5">

 <li>Convert the values stored in <strong>in_time</strong> and <strong>out_time</strong> to the <strong>datetime</strong> format, so as to compute the time differences.</li>

</ol>

in_time = pd.to_datetime(in_time) out_time = pd.to_datetime(out_time) The <strong>to_datetime()</strong> function returns a datetime object of the time string.




<ol start="6">

 <li>Then, we subtract the OUT time by the IN time to get the working hours. The results of the subtraction are in <strong>timedelta</strong> object format.</li>

</ol>




<ol start="7">

 <li>Use the <strong>astype()</strong> function with the option <strong>timedelta64[m]</strong> to retrieve the <u>number of minutes</u> representing in float format.</li>

</ol>

working_hours = out_time – in_time

working_hours = working_hours.astype(‘timedelta64[m]’)




<ol start="8">

 <li>Convert the working hours from minutes to hours and round the results down to two decimal places.</li>

</ol>

Add the working hours to the records data frame as a new column named “Working Hours”.

working_hours = round(working_hours/60, 2) records[‘Working Hours’] = working_hours




<ol start="9">

 <li>Delete the “Working Hours (In &amp; Out)” column because it is no longer necessary.</li>

</ol>

records.drop(‘Working Hours (In &amp; Out)’, axis = 1, inplace = True)




We now have the records data frame as follows:

<table width="0">

 <tbody>

  <tr>

   <td width="30"> </td>

   <td width="85"><strong>Date </strong></td>

   <td width="90"><strong>Staff Name </strong></td>

   <td width="129"><strong>Department </strong></td>

   <td width="48"><strong>In </strong></td>

   <td width="55"><strong>Out </strong></td>

   <td width="113"><strong>Working Hours </strong></td>

  </tr>

  <tr>

   <td width="30"><strong>0 </strong></td>

   <td width="85">6/15/2018</td>

   <td width="90">Jacky Lee</td>

   <td width="129">IT</td>

   <td width="48">6:12</td>

   <td width="55">20:44</td>

   <td width="113">14.53</td>

  </tr>

  <tr>

   <td width="30"><strong>1 </strong></td>

   <td width="85">6/15/2018</td>

   <td width="90">Joyce Ho</td>

   <td width="129">Human Resources</td>

   <td width="48">8:26</td>

   <td width="55">19:51</td>

   <td width="113">11.42</td>

  </tr>

  <tr>

   <td width="30"><strong>2 </strong></td>

   <td width="85">6/15/2018</td>

   <td width="90">Ivy Chu</td>

   <td width="129">Sales</td>

   <td width="48">8:29</td>

   <td width="55">18:44</td>

   <td width="113">10.25</td>

  </tr>

  <tr>

   <td width="30"><strong>3 </strong></td>

   <td width="85">6/15/2018</td>

   <td width="90">David Chan</td>

   <td width="129">Purchasing</td>

   <td width="48">8:33</td>

   <td width="55">19:50</td>

   <td width="113">11.28</td>

  </tr>

  <tr>

   <td width="30"><strong>4 </strong></td>

   <td width="85">6/15/2018</td>

   <td width="90">Gary Yuen</td>

   <td width="129">Logistics</td>

   <td width="48">8:34</td>

   <td width="55">19:45</td>

   <td width="113">11.18</td>

  </tr>

  <tr>

   <td width="30"><strong>… </strong></td>

   <td width="85">…</td>

   <td width="90">…</td>

   <td width="129">…</td>

   <td width="48">…</td>

   <td width="55">…</td>

   <td width="113">…</td>

  </tr>

 </tbody>

</table>

<h1>Computing the Daily Wages</h1>

To compute the daily wages, we need the hourly salary rate of each staff. After that we use the following formula to compute the daily wages.




<ol start="10">

 <li>Look up the hourly salary rates for individual staffs by merging two data frames.</li>

</ol>

records = pd.merge(records, hourly_rate, how=’left’, left_on=’Staff Name’, right_index=True)

In the statement above, we provide the <strong>merge()</strong> function many things including:

<ul>

 <li><strong>records, hourly_rate</strong> – two source data frames. The <strong>records</strong> data frame is on left-hand side, the <strong>hourly_rate</strong> data frame is on right-hand side.</li>

 <li><strong>how=’left’</strong> – use <em>left join</em> method to merge the data. The merged result includes all rows of the left-hand side but some rows of right-hand side may not be included.</li>

 <li><strong>left_on=’Staff Name’</strong> – the left-hand side, use the ‘Staff Name’ column for merging.</li>

 <li><strong>right_index=True</strong> – the right-hand side, use the index column for merging.</li>

</ul>

After the merging, the records data frame stores the data as follows:

<table width="0">

 <tbody>

  <tr>

   <td width="30"> </td>

   <td width="84"><strong>Date </strong></td>

   <td width="90"><strong>Staff Name </strong></td>

   <td width="102"><strong>Department </strong></td>

   <td width="68"><strong>In </strong></td>

   <td width="68"><strong>Out </strong></td>

   <td width="68"><strong>Working Hours </strong></td>

   <td width="68"><strong>Salary Rate  </strong><strong>(per hour) </strong></td>

  </tr>

  <tr>

   <td width="30"><strong>0 </strong></td>

   <td width="84">6/15/2018</td>

   <td width="90">Jacky Lee</td>

   <td width="102">IT</td>

   <td width="68">6:12</td>

   <td width="68">20:44</td>

   <td width="68">14.53</td>

   <td width="68">60</td>

  </tr>

  <tr>

   <td width="30"><strong>1 </strong></td>

   <td width="84">6/15/2018</td>

   <td width="90">Joyce Ho</td>

   <td width="102">HumanResources</td>

   <td width="68">8:26</td>

   <td width="68">19:51</td>

   <td width="68">11.42</td>

   <td width="68">70</td>

  </tr>

  <tr>

   <td width="30"><strong>2 </strong></td>

   <td width="84">6/15/2018</td>

   <td width="90">Ivy Chu</td>

   <td width="102">Sales</td>

   <td width="68">8:29</td>

   <td width="68">18:44</td>

   <td width="68">10.25</td>

   <td width="68">55</td>

  </tr>

  <tr>

   <td width="30"><strong>3 </strong></td>

   <td width="84">6/15/2018</td>

   <td width="90">David Chan</td>

   <td width="102">Purchasing</td>

   <td width="68">8:33</td>

   <td width="68">19:50</td>

   <td width="68">11.28</td>

   <td width="68">65</td>

  </tr>

  <tr>

   <td width="30"><strong>4 </strong></td>

   <td width="84">6/15/2018</td>

   <td width="90">Gary Yuen</td>

   <td width="102">Logistics</td>

   <td width="68">8:34</td>

   <td width="68">19:45</td>

   <td width="68">11.18</td>

   <td width="68">75</td>

  </tr>

  <tr>

   <td width="30"><strong>… </strong></td>

   <td width="84">…</td>

   <td width="90">…</td>

   <td width="102">…</td>

   <td width="68">…</td>

   <td width="68">…</td>

   <td width="68">…</td>

   <td width="68">…</td>

  </tr>

 </tbody>

</table>

<ol start="11">

 <li>Compute the daily wages using the columns “Working Hours” and “Salary Rate (per hour)”.</li>

</ol>




records[‘Daily Wage’] = records[‘Salary Rate (per hour)’] * records[‘Working Hours’]

<ol start="12">

 <li>Sort the rows by “Working Hours” and “Staff Name” in <strong>descending</strong> The result is stored back in the <strong>records</strong> data frame.</li>

</ol>




records.sort_values(by = [‘Working Hours’, ‘Staff Name’], ascending = False, inplace = True)

<h1>Creating Summaries using Pivot Table</h1>

Pivot Table is a very useful and powerful feature which can be used to summarize, analyze, explore and present our data. We could use a Pivot Table to produce meaningful information from a large table of information, such as:

<ul>

 <li>The total and daily working hours for each staff</li>

 <li>The total and average working hours for each staff in each department</li>

 <li>The total and average working hours of all staff in each department</li>

 <li>The total and average working hours for each staff</li>

</ul>




<ol start="13">

 <li>Create a pivot table named table1 that contains the total working hours for each staff.</li>

</ol>




table1 = pd.pivot_table(records, values = ‘Working Hours’, index = ‘Staff Name’, aggfunc = np.sum)




<ol start="14">

 <li>Create a pivot table named table2 that contains the daily working hours grouped by the departments.</li>

</ol>




<table width="0">

 <tbody>

  <tr>

   <td width="368">table2 = pd.pivot_table(records, values = ‘Working columns=’Date’,  aggfunc = np.sum)</td>

   <td width="210">Hours’, index = ‘Department’,</td>

  </tr>

 </tbody>

</table>

<ol start="15">

 <li>Create a pivot table named table3 that contains the total working hours and the total wages, average wages, minimum and maximum daily wages for each staff.</li>

</ol>




table3 = pd.pivot_table(records, values = [‘Working Hours’, ‘Daily Wage’], index = ‘Staff Name’,      aggfunc = {‘Working Hours’:[min, max, np.mean, np.sum], ‘Daily Wage’:np.sum})




<ol start="16">

 <li>Create a pivot table named table4 that contains the total wages and working hours grouped by the departments. In addition, we rename the columns by using the <strong>rename()</strong></li>

</ol>




table4 = pd.pivot_table(records, values = [‘Daily Wage’, ‘Working Hours’], index = ‘Department’,      aggfunc = np.sum)




table4.rename(columns={‘Daily Wage’:’Total salary’, ‘Working Hours’:’Total Working Hours’}, inplace = True)

<h1>Creating Charts</h1>

After generating the pivot tables, we could create charts to show the distribution of the working hours among all departments, total working hours of each staff in the period, etc. <strong>Pandas’ DataFrame</strong> provides the <strong>plot()</strong> function for creating charts.

The <strong>plot()</strong> function provides many options for us to create different charts. The commonly used options (parameters) are:




<ul>

 <li>x: label of x axis. If not specify, the index column will be used.</li>

 <li>y: label of y axis.</li>

 <li>kind: the kind of the chart including:

  <ul>

   <li>‘line’ : line plot (default) o ‘bar’ : vertical bar plot o ‘barh’ : horizontal bar plot o ‘hist’ : histogram o ‘box’ : boxplot</li>

   <li>‘kde’ : Kernel Density Estimation plot o ‘density’ : same as ‘kde’ o ‘area’ : area plot o ‘pie’ : pie plot o ‘scatter’ : scatter plot o ‘hexbin’ : hexbin plot</li>

  </ul></li>

 <li>ax: matplotlib axes object.</li>

 <li>title: add the title to the chart.</li>

 <li>legend: hide, show or reverse the legend.</li>

</ul>

<ol start="17">

 <li>Create a vertical bar chart using the data of <strong>table1</strong> with parameters – <em>kind = ‘bar’ and legend = False</em>:</li>

</ol>




table1.plot(kind=’bar’, legend = False)













<ol start="18">

 <li>Create a horizontal bar chart using the data of <strong>table2</strong> with parameters – title <em>= ‘Working Hours in Different Departments’ and kind = ‘barh’</em>.</li>

</ol>




table2.plot(title = ‘Working Hours in Different Departments’, kind = ‘barh’)







<ol start="19">

 <li>Create a pie chart using the data in the column “Total Working Hours” of <strong>table4</strong> with parameters –<em> kind = ‘pie’, y = ‘Total Working Hours’ and legend = False</em>:</li>

</ol>




table4.plot(title = ‘Working Hours of Different Departments’, kind = ‘pie’, y = ‘Total Working Hours’, legend = False)







<h1>Exercise</h1>

<h2>Exercise 1 – Calculating Discounted Amount</h2>

Write a program to load the data from the data file named <strong>payment_records.csv</strong> to a data frame. Then, add two additional columns – discount rate and discounted amount. The discount rate can be found in another data file named <strong>vip.csv</strong>. The discounted amount can be calculated using the following formula:

”                #<sub>$%&amp;’() </sub>=*                   ∗        ”          _

You need to use the “VIP Level” to match the discount rate for each record. And, save the data to another file named result.csv.




<h2>Exercise 2 – Pivot Table about Payment Method</h2>

Write a program to create a pivot table to show the sum of amounts in different payment methods (Mastercard, VISA, and Cash).







<h1>More Visualization Practice using Matplotlib</h1>

You may find that there may be some problems on the charts, such as the bars go behind the legend, text are overlapped, etc. To solve the problems and create some advanced charts, we use the library <strong>Matplotlib</strong> too. Let’s explore the common chart functions include <strong>pie()</strong>, <strong>bar()</strong> and <strong>barh()</strong>.

<h2>Pie Chart</h2>

The following example creates a pie chart with percentage labels using <strong>Mathplotlib’s pie()</strong>. The parameters of the <strong>pie()</strong> function used in the example means:

<ul>

 <li>data: the chart data.</li>

 <li>labels: the text label shown for each slice.</li>

 <li>autopct: the value label with a formatted string “%.0f%%”. In the formatted string, the string “%.0f” represents a float with no decimal place. The double-percent signs are used to add a percent sign to the label. With the string “%.0f%%”, we can show the float numbers as a percentage with no decimal place.</li>

</ul>

import matplotlib.pyplot as plt

plt.pie(table4[‘Total salary’], labels=table4.index, autopct=’%.0f%%’) plt.title(‘Working Hours of Different Departments’) plt.show()

Horizontal Bar Chart

The following example creates a horizontal bar chart using <strong>Matplotlib’s barh()</strong> function. We pass two parameters to the <strong>barh()</strong> function as chart data and <strong>table4.index</strong> is used as Y axis labels, <strong>table4[‘Total salary’]</strong> is used as X axis labels.

plt.barh(table4.index, table4[‘Total salary’]) plt.title(‘Working Hours of Different Departments’) plt.show()

Vertical Bar Chart

The following example creates a vertical bar chart using <strong>Matplotlib’s bar()</strong> function. We pass two parameters to the <strong>bar()</strong> function as chart data and <strong>table4.index</strong> is used as X axis labels, <strong>table4[‘Total salary’]</strong> is used as Y axis labels.

plt.bar(table4.index, table4[‘Total salary’]) plt.xticks(rotation=’vertical’)

plt.title(‘Working Hours of Different Departments’) plt.show()

MatPlotlib Together with Pandas’ Plot

Creating a chart below using <strong>MatPlotlib</strong> along is not easy. But, the legend overlaps with the bars if we create a chart using the <strong>Pandas’ plot()</strong> function. Actually, the problem can be solved by using both <strong>MatPlotlib</strong> and <strong>Pandas’ plot()</strong> function.




The procedure is as follows:

<ol>

 <li>Create a figure using Matplotlib.</li>

</ol>

fig = plt.figure()




<ol start="2">

 <li>Create a bar chart using Pandas’ plot() function with the axes of the figure. The <strong>gca()</strong> function returns the current axes of the figure.</li>

</ol>

table2.plot(title = ‘Working Hours in Different Departments’, kind = ‘barh’, ax=fig.gca())




<ol start="3">

 <li>Move the legend outside the chart area.</li>

</ol>

plt.legend(bbox_to_anchor=(1,1))




<ol start="4">

 <li>Show the chart.</li>

</ol>

plt.show()














