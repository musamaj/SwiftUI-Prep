# SwiftUI Grid Examples

This repository demonstrates various ways to use SwiftUI's `Grid` view for creating flexible and dynamic layouts.

## Overview

SwiftUI's `Grid` provides a powerful way to organize views in a two-dimensional grid structure. This repository showcases basic and advanced usage of `Grid`, including column spanning, alignment, and more.

## Features

* **Basic Grid Layout:** Demonstrates how to create a simple grid with rows and columns.
* **Column Spanning:** Shows how to make a view span multiple columns using `gridCellColumns(Int)`.
* **Alignment Control:** Explains how to use `gridCellAnchors(Alignment)`, `gridColumnAlignment(HorizontalAlignment)`, and `gridRowAlignment(VerticalAlignment)` for precise alignment.
* **Unsized Axes:** Shows how to use `gridCellUnsizedAxes(Axis.Set)` to disable an element from affecting size of that axis of the grid.
* **Customization:** Provides examples of customizing grid appearance with modifiers.

## Getting Started

1.  Clone the repository:

    ```bash
    git clone [repository URL]
    ```

2.  Open the project in Xcode.
3.  Run the app on a simulator or device.

## Examples

### Basic Grid

```swift
import SwiftUI

struct BasicGrid: View {
    var body: some View {
        Grid {
            GridRow {
                Text("Item 1")
                Text("Item 2")
                Text("Item 3")
            }
            GridRow {
                Text("Item 4")
                Text("Item 5")
                Text("Item 6")
            }
        }
    }
}

import SwiftUI

struct ColumnSpanning: View {
    var body: some View {
        Grid {
            Text("Header").gridCellColumns(3)
            GridRow {
                Text("Item 1")
                Text("Item 2")
                Text("Item 3")
            }
        }
    }
}

import SwiftUI

struct AlignmentExample: View {
    var body: some View {
        Grid {
            GridRow {
                Text("Left").gridColumnAlignment(.leading)
                Text("Center").gridColumnAlignment(.center)
                Text("Right").gridColumnAlignment(.trailing)
            }
        }
    }
}

import SwiftUI

struct UnsizedAxesExample: View {
    var body: some View {
        Grid {
            GridRow {
                Text("Item 1").border(.red)
                Text("Item 2").border(.blue).gridCellUnsizedAxes(.horizontal)
                Text("Item 3").border(.green)
            }
        }
    }
}
