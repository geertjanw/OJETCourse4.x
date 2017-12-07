# Lesson 2 - Part 4

Homework research:

   * Developer Guide: [Integrating REST Services](https://docs.oracle.com/middleware/jet410/jet/developer/GUID-2ED73C1B-10A6-43C9-AF55-ABF1A8990B9F.htm#JETDG171) 
   * [REST Countries](https://restcountries.eu/)
   * Developer Guide: [Dealing With Cross-Origin Resource Sharing (CORS)](https://docs.oracle.com/middleware/jet410/jet/developer/GUID-07537BBC-2EE0-4C82-B815-569D9F5A05E9.htm#JETDG-GUID-07537BBC-2EE0-4C82-B815-569D9F5A05E9)
   * [jQuery](https://jquery.com/)
   * Cookbook: [Oracle JET Common Model](http://www.oracle.com/webfolder/technetwork/jet/jetCookbook.html?component=home&demo=rootFramework_childCommonModel)
   * Developer Guide: [Using the Common Model and Collection API](https://docs.oracle.com/middleware/jet410/jet/developer/GUID-2549871C-9658-4035-B461-A9136554BE74.htm#JETDG166)

Source code:

```js #button { border: none; }
define(['ojs/ojcore'], function (oj) {
    var CountryFactory = {
        resourceUrl: 'https://restcountries.eu/rest/v2/all',
        // Create a single country instance:
        createCountryModel: function () {
            var Country = oj.Model.extend({
                urlRoot: this.resourceUrl, 
                idAttribute: "name"
            });
            return new Country();
        },
        // Create a country collection:
        createCountryCollection: function () {
            var Countries = oj.Collection.extend({
                url: this.resourceUrl, 
                model: this.createCountryModel()
            });
            return new Countries();
        }
    };
    return CountryFactory;
});
```

```js #button { border: none; }
define(['ojs/ojcore', 'knockout', 'jquery', 'factories/CountryFactory',
    'ojs/ojtable', 'ojs/ojcollectiontabledatasource'],
    function (oj, ko, $, CountryFactory) {
        var viewModel = {
            countryCollection: CountryFactory.createCountryCollection(),
            datasource: ko.observable(),
            // Called each time the view is shown to the user:
            initialize: function () {
                this.datasource(new oj.CollectionTableDataSource(this.countryCollection));
                this.countryCollection.fetch();
            }
        };
        return viewModel;
    }
);
```

```html #button { border: none; }
<div class="oj-hybrid-padding">
    <h1>Customers Content Area</h1>
    <oj-table id='table' aria-label='Departments Table'
              data='[[datasource]]' 
              columns-default.sortable='disabled' 
              columns='[
                 {"headerText": "Name", "field": "name"},
                 {"headerText": "Population", "field": "population"},
                 {"headerText": "Capital", "field": "capital"}]'>
    </oj-table>
</div>
```