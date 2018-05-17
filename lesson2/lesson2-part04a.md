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
    chart-type="bar" 
    chart-label="Car Sales"></my-chart>
```

In the browser, you should now see that the data is being loaded from a file. 

### Visualizing Data from a REST Endpoint

https://apex.oracle.com/pls/apex/oraclejet/emp/

https://apex.oracle.com/pls/apex/oraclejet/m/emp/

## Day 3   




