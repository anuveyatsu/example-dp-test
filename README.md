This is an example dataset, that demonstrates how to build visualizations using the "Vega Graph Spec". We are using lifelines of the first 5 presidents of the US - one of the examples from [vega editor][editor] - and displaying it here, on DataHub with small modifications in vega-spec.

## Views

We assume that you are familiar with what [datapackage.json][datapackage.json] is and its specifications.

To create graphs for your tabular data, the `datapackage.json` should include the `views` attribute that is used for visualizations.

If you are familiar with [Vega][vega] specifications, you probably would like to use all its features. To use it, inside `views` you should set `specType` to `"vega"` and define some graph specifications in `spec` property. See below for more details.

### Vega Graph Specifications

This is the `views` property that is used for this page:

```
{
  ...,
  "views": [
    {
      "name": "demo-datapackage-vega",
      "title": "Lifelines of first 5 US presidents",
      "resources": ["people", "events"],
      "specType": "vega",
      "spec": {
        "width": 800,
        "height": 200,
        "data": [
          {
            "name": "people"
          },
          {
            "name": "events",
            "format": {"type":"json", "parse":{"when":"date"}}
          }
        ],
        "scales": [
          {
            "name": "y",
            "type": "ordinal",
            "range": "height",
            "domain": {"data": "people", "field": "label"}
          },
          {
            "name": "x",
            "type": "time",
            "range": "width",
            "round": true,
            "nice": "year",
            "domain": {"data": "people", "field": ["born", "died"]}
          }
        ],
        "axes": [
          {"type": "x", "scale": "x"}
        ],
        "marks": [
          {
            "type": "text",
            "from": {"data": "events"},
            "properties": {
              "enter": {
                "x": {"scale": "x", "field": "when"},
                "y": {"value": -10},
                "angle": {"value": -25},
                "fill": {"value": "#000"},
                "text": {"field": "name"},
                "font": {"value": "Helvetica Neue"},
                "fontSize": {"value": 10}
              }
            }
          },
          {
            "type": "rect",
            "from": {"data": "events"},
            "properties": {
              "enter": {
                "x": {"scale": "x", "field": "when"},
                "y": {"value": -8},
                "width": {"value": 1},
                "height": {"field": {"group": "height"}, "offset": 8},
                "fill": {"value": "#888"}
              }
            }
          },
          {
            "type": "text",
            "from": {"data": "people"},
            "properties": {
              "enter": {
                "x": {"scale": "x", "field": "born"},
                "y": {"scale": "y", "field": "label", "offset": -3},
                "fill": {"value": "#000"},
                "text": {"field": "label"},
                "font": {"value": "Helvetica Neue"},
                "fontSize": {"value": 10}
              }
            }
          },
          {
            "type": "rect",
            "from": {"data": "people"},
            "properties": {
              "enter": {
                "x": {"scale": "x", "field": "born"},
                "x2": {"scale": "x", "field": "died"},
                "y": {"scale": "y", "field": "label"},
                "height": {"value": 2},
                "fill": {"value": "#557"}
              }
            }
          },
          {
            "type": "rect",
            "from": {"data": "people"},
            "properties": {
              "enter": {
                "x": {"scale": "x", "field": "enter"},
                "x2": {"scale": "x", "field": "leave"},
                "y": {"scale": "y", "field": "label", "offset":-1},
                "height": {"value": 4},
                "fill": {"value": "#e44"}
              }
            }
          }
        ]
      }
    }
  ]
}
```

<br>

You can use almost the same specifications inside `spec` attribute, that are used for setting the Vega graphs. Only difference is that in `data` property, `url` and `path` attributes are moved out. Instead we use `name` attribute to reference to a resource. Note, how we created a new object inside `data` property - `{"name": "flights-airport"}` to reference it to a resource (this names are identical to names of resources):

```
  ...
  "spec": {
    ...
    "data": [
      {
        "name": "people"
      },
      {
        "name": "events",
        ...
      }
    ],
  }
```

Outside of `spec` attribute there are some other important parameters to note:

<table class="table table-bordered table-striped resource-summary">
  <thead>
   <tr>
     <th>Attribute</th>
     <th>Type</th>
     <th>Description</th>
   </tr>
  </thead>
  <tbody>
    <tr>
      <th>name</th>
      <td>String</td>
      <td>Unique identifier for view within list of views.</td>
    </tr>
    <tr>
      <th>title</th>
      <td>String</td>
      <td>Title for the graph.</td>
    </tr>
    <tr>
      <th>resources</th>
      <td>Array</td>
      <td>Data sources for this spec. It can be either resource name or index. By default it is the first resource.</td>
    </tr>
    <tr>
      <th>specType</th>
      <td>String</td>
      <td>Available options: simple, vega, plotly <strong>(Required)</strong>.</td>
    </tr>
  </tbody>
</table>

[vega]: https://vega.github.io/vega/
[editor]: https://vega.github.io/vega-editor/?mode=vega&spec=lifelines
[datapackage.json]: http://specs.frictionlessdata.io/data-package/
