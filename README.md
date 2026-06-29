# How-to-Create-a-Custom-Text-and-Image-Column-in-a-Flutter-DataTable

In this article, we will show you how to create a custom text and image column in a [Flutter DataTable](https://www.syncfusion.com/flutter-widgets/flutter-datagrid).

Initialize the [SfDataGrid](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/SfDataGrid-class.html) widget with all the required configurations. To implement a custom column that displays both text and images, extend the functionality of the [DataGridSource](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridSource-class.html) by introducing a helper method named `_getImageForColumn`. This method should return an [ImageProvider](https://api.flutter.dev/flutter/painting/ImageProvider-class.html) based on the column name or cell data, enabling dynamic and conditional image mapping for specific columns.

In the [buildRow](https://pub.dev/documentation/syncfusion_flutter_datagrid/latest/datagrid/DataGridSource/buildRow.html) method, construct each cell using a [Container](https://api.flutter.dev/flutter/widgets/Container-class.html) that embeds a [Row](https://api.flutter.dev/flutter/widgets/Row-class.html) widget to align multiple child elements horizontally. Inside the Row, render the textual content using a [Text](https://api.flutter.dev/flutter/widgets/Text-class.html) widget, followed by an [Image](https://api.flutter.dev/flutter/widgets/Image-class.html) widget to display the corresponding image.

To ensure flexibility and performance, include a conditional check before rendering the image widget. The image should only be added to the widget tree if a valid `ImageProvider` is returned by `_getImageForColumn`. This approach prevents unnecessary widget creation and allows developers to tailor the visibility logic based on specific business requirements or data conditions.

```dart
  @override
  DataGridRowAdapter buildRow(DataGridRow row) {
    return DataGridRowAdapter(
      cells: row.getCells().map<Widget>((e) {
        // Retrieve mapped image provider for the current column.
        final imageProvider = _getImageForColumn(e.columnName);

        return Container(
          alignment: Alignment.center,
          padding: EdgeInsets.all(8.0),
          child: Row(
            children: [
              // Text
              Expanded(
                child: Text(
                  e.value.toString(),
                  textAlign:
                      e.columnName == 'id' ||
                          e.columnName ==
                              'name' // Text Alignment as required
                      ? TextAlign.left
                      : TextAlign.center,
                  overflow: TextOverflow.ellipsis,
                ),
              ),

              /// Conditional rendering:
              /// Image widget is instantiated only when a valid provider exists.
              if (imageProvider != null) ...[
                const SizedBox(width: 8), // spacing between text and image.
                Image(
                  image: imageProvider,
                  width: 30,
                  height: 30,
                ), // Used Image widget to display image.
              ],
            ],
          ),
        );
      }).toList(),
    );
  }

```

You can download this example on [GitHub]().
