# Workshop Scenarios

## Three Day Oracle JET Workshop

#### Day 1

   * Getting Started
   * Working with Oracle JET Components
   * Working with Oracle JET Composite Components
   
#### Day 2
   
   * Visualizing Data from a File
   * Visualizing Data from a REST Endpoint
   * Adding CRUD Functionality
   
#### Day 3 

   * Miscellaneous Topics
   
## Day 1   

### Getting Started

1. Follow the instruction on the Getting Started page to install the ojet-cli:

http://www.oracle.com/webfolder/technetwork/jet/globalGetStarted.html

2. Run the following in the terminal:

```js #button { border: none; }
ojet create BankAnalyzer --template=navdrawer
```
3. Run the following in the terminal and look in the browser:

4. Open your editor and eplore the source structure.

5. Make a change in the 'Dashboard.html' file and notice what happens in the browser, without needing to refresh.

### Working with Oracle JET Components

1. Explore the Oracle JET Cookbook.

2. Especially explore the data visualization use cases:

http://www.oracle.com/webfolder/technetwork/jet/jetCookbook.html?component=dataVisualizations&demo=gallery

3. Take a look at the Bar Chart:

http://www.oracle.com/webfolder/technetwork/jet/jetCookbook.html?component=barChart&demo=default

4. Look at the JS documentation, the description, variations, and tweak the code.

5. Copy the HTML into the Dashboard View, the JavaScript into the Dashboard ViewModel, and reference the 'ojs/ojchart' in the define block of the Dashboard ViewModel. Look in the browser and see the Bar Chart is displayed.

6. Change the 'bar' type to 'pie' by hand and then let the user do it by combining with a Combobox One:

http://www.oracle.com/webfolder/technetwork/jet/jetCookbook.html?component=comboboxOne&demo=single

In this scenario, also discuss the square braces vs. curly braces.

### Working with Oracle JET Composite Components

Let's now take the chart and the combobo and turn them into a reusable CCA component.

#### Getting Started

1. In the Terminal, in the root of your project, run the following:

```js #button { border: none; }
ojet create component my-chart
```

2. Take a look at your source structure and find the new 'my-chart' CCA component.

3. Load the loader into the ViewModel, declare the new custom element in the View, then look in the browser and notice the message from the CCA component.

4. Move the chart and the combobox into the 'my-chart' CCA component.

5. Declare the 'my-chart' custom element a few more times and notice that you now see multiple charts.

```html #button { border: none; }
<my-chart></my-chart>
<my-chart></my-chart>
```

#### Setting Properties

1. In 'component.json', add a 'chartType' property:

```js #button { border: none; }
"properties": {
    "chartType": {
        "type": "string"
    }
},
```

2. In 'dashboard.html', use the property in your 'my-chart' custom elements:

```html #button { border: none; }
<my-chart chart-type="bar"></my-chart>
<my-chart chart-type="pie"></my-chart>
```

3. In 'my-chart-viewmodel.js', create a new Knockout property that is initialized by the custom element's context, i.e., by 'bar' and by 'pie' in the two examples above:

```js #button { border: none; }
self.chartType = ko.observable(context.properties.chartType);
```

4. In 'my-chart-view.html', change the value of the combobox to '{{chartType}}' and the type of the chart to '[[chartType]]'.

5. Replace some of the other properties in the CCA component so that they can be set from the Dashboard module.

#### Using Arrays in Properties

1. In 'component.json', define a new property, this time an 'array', to work with the 'series' property of the chart:

```js #button { border: none; }
"properties": {
    "chartType": {
        "type": "string"
    },
    "chartData": {
        "type": "array"
    }
},
```

2. In 'dashboard.html', use the property, by referencing a display-only variable to be defined in the 'dashboard.js':

```html #button { border: none; }
<my-chart
    chart-data="[[carsales]]"
    chart-type="bar"></my-chart>
```

3. In 'dashboard.js', define the variable referenced above:

```js #button { border: none; }
self.carsales = [
    {name: "Honda", items: [12, 24]},
    {name: "Volvo", items: [95, 39]},
    {name: "Mazda", items: [16, 52]},
    {name: "Mercedes", items: [32, 26]},
    {name: "Jaguar", items: [82, 36]}];
```

4. In 'my-chart-viewModel.js', set a property that is defined by the 'my-chart' custom element:

```js #button { border: none; }
self.chartData = context.properties.chartData;
```

5. Reference the property above in the 'my-chart-view.html' file, in the 'series' property of the chart:

```html #button { border: none; }
series="[[chartData]]"
```

In the browser, you should see that your data can now be defined per 'my-chart' custom element, rather than being hardcoded in the CCA component.

## Day 2

Today, we will focus on displaying and manipulating data.

### Visualizing Data from a File

In the first scenario, we move our hardcoded data from the CCA component into a JSON file. From there, we load the data into our ViewModel, parse it, and then push it into a variable referenced in our view.

1. Move the data into a file named 'carsales.json', in 'js/data'. Here's what its content should look like:

```js #button { border: none; }
[
    {"name":"Honda", "items": [42, 34]},
    {"name":"Volvo", "items": [55, 30]},
    {"name":"Fiat", "items": [36, 50]},
    {"name":"Mercedes", "items": [22, 46]},
    {"name":"Jaguar", "items": [10, 46]}
]
```

2. Load the file in the 'define' block:

```js #button { border: none; }
'text!../data/carsales.json'
```

3. Reference the file in the callback function in the same position as the file in the 'define' block:

```js #button { border: none; }
carSalesFile
```

4. Now parse the file and push into the 'carsales' variable:

```js #button { border: none; }
self.carsales = [];
var carSalesContent = JSON.parse(carSalesFile);
for (var i = 0; i < carSalesContent.length; i++) {
    var carSale = carSalesContent[i];
    self.carsales.push({
        name: carSale["name"],
        items: carSale["items"]
    });
}
```

5. Reference the 'carsales' variable as display-only in the 'chart-data' attribute of the 'my-chart' CCA component:

```html #button { border: none; }
<my-chart
    chart-data="[[carsales]]"
    chart-type="bar"></my-chart>
```

In the browser, you should now see that the data is being loaded from a file. 

### Visualizing Data from a REST Endpoint

We'll use these REST endpoints, one for large resolution and the other for small devices, e.g., mobile phones, which have less real estate so we should make a smaller subset of data available:

https://apex.oracle.com/pls/apex/oraclejet/emp/

https://apex.oracle.com/pls/apex/oraclejet/m/emp/

### Adding CRUD Functionality

## Day 3   

### Internationalization

### Explore the Oracle JET Website oraclejet.org





