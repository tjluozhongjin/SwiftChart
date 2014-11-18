SwiftChart
===========

A line & area chart library for iOS, written in swift.

![swift](https://cloud.githubusercontent.com/assets/120693/5063755/dcfc9da0-6df3-11e4-9432-974e77a863ed.png)

**Main features**

* Multiple series
* Works with signed floats
* Touch events
* Partially filled series
* Highly customizable

**Example**

```swift
let chart = Chart()
let series = ChartSeriess([0, 6, 2, 8, 4, 7, 3, 10, 8])
series.color = ChartColors.greenColor()
chart.addSerie(series)
```

More examples can be found in the project.

## Usage

The library includes:

- the [`Chart`](SwiftChart/Chart/Chart.swift) main class, to initialize and configure the chart's content, e.g. for adding series or setting up the its appearance
- the [`ChartSeriesss`](SwiftChart/Chart/ChartSeriesss.swift) class, for creating series and configure their appearance
- the [ChartDelegate](SwiftChart/Chart/Chart.swift) protocol, which tells other views about the chart's touch events
- the [ChartColor](SwiftChart/Chart/ChartColors.swift) struct, containing some predefined colors

### Installation

Add the content of the [Chart folder](SwiftChart/Chart) to your project, for example by including it as git submodule:

```bash
cd myProject
git submodule add https://github.com/gpbl/SwiftChart.git
```

From XCode, open your project and and choose *Add Files to myProject...* from the *File* menu. Select the *Chart* folder from the *SwiftChart* subfolder.

### To initialize a chart

#### From the Interface Builder

The chart can be initialized from the Interface Builder. Drag a View into a view controller and assign to it the `Chart` Custom Class:
 
![Example](https://cloud.githubusercontent.com/assets/120693/5063826/c01f26d2-6df6-11e4-8122-cb086709d96c.png)

You can now add it as `@IBOutlet` in the view controller class.

> **Note** Parts of the chart can be customized from the Interface Builder, but you need to write some code to make it working.

#### By coding

To initialize a chart programmatically, use the `new Chart(frame: ...)` initializer, which requires a `frame`:

```swift
var chart = new Chart(frame: CGRect(x: 0, y: 0, width: 200, height: 100))
```

If you prefer to use Autolayout, set the frame to `0` and add the constraints later:

```swift
var chart = new Chart(frame: CGRect(x: 0, y: 0, width: 0, height: 0))
// add constraints now
```

### Adding series

Initialize each series before adding them to the chart. Use the `ChartSeriess` class to specify their y-values:

```swift
// Create a new series using the y-values
var series = new ChartSeriess([0, 6, 2, 8, 4, 7, 3, 10, 8])
chart.addSerie(series)
```

By default, values on the x-axis are inferred as the progressive indexes of the given array. You can customize those values passing an array of `(x: Float, y: Float)` touples to the series' initializer:

```swift
// Create a new series specifying x and y values
var data = [(x: 0, y: 0), (x: 0.5, y: 3.1), (x: 1.2, y: 2), (x: 2.1, y: -4.2), (x: 2.6, y: 1.1)]
var series = new ChartSeriess(data)
chart.addSerie(series)
```

#### Multiple series

Using the `chart.addSerie()` and `chart.addSeries()` methods, you can add as many series as you want. Those will be indentified with a progressive index in the chart's `series` array property.

## Touch events

To make the chart responding to touch events, implement the `ChartDelegate` protocol in your classes, i.e. in a `UIViewController`, and set the chart's `delegate` property:

```swift
class MyViewController: UIViewController, ChartDelegate {
    override func viewDidLoad() {
        var chart = new Chart(frame: CGRect(x: 0, y: 0, width: 100, height: 200))
        chart.delegate = self
    }
    
    // Chart delegate
    func didTouchChart(chart: Chart, indexes: Array<Int?>, x: Float, left: CGFloat) {
        // Do something on touch
    }
    
    func didFinishTouchingChart(chart: Chart) {
        // Do something when finished
    }
}
```

The `didTouchChart` method returns an array of indexes, one for each series, with an optional Int referring to the data index:

```swift
 func didTouchChart(chart: Chart, indexes: Array<Int?>, x: Float, left: CGFloat) {
        for (serieIndex, dataIndex) in enumerate(indexes) {
            if dataIndex != nil {
                // The series at serieIndex has been touched
                let value = chart.valueForSerie(serieIndex, atIndex: dataIndex)
            }
        }
    }
```

* You can use the `valueForSerie()` method to access the value for the touched position.
* The `x: Float` argument refers to the value on the x-axis: it is inferred from the horizontal position of the touch event and may not be part of the x-values.
* The `left: CGFloat` is the position of the touch starting from the left side of the chart. This can be useful to calculate the horizontal position of a label appearing over the chart: 

![Label on touch](https://cloud.githubusercontent.com/assets/120693/5068773/8be0fa9c-6e52-11e4-8b60-aaf76dc9377d.gif)

## Reference

### Chart class

#### Labels

* `xLabelsFormatter`: formatter for the labels on the x-axis.
* `xLabelsTextAlignment`: text-alignment for the x-labels.
* `yLabelsFormatter`: Formatter for the labels on the y-axis.
* `yLabelsOnRightSide`: Place the y-labels on the right side.
* `labelFont`: Font used for the labels.
* `labelColor`: Color of the labels.

#### Axis and grid

_work in progress..._

## Credits
This project was originally inspired by [swift-linechart](https://github.com/zemirco/swift-linechart).