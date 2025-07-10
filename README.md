# Swift UI - QA
Consist important interview Questions &amp; Answer's

## Qestion 1:  SwiftUI - Automatic Accessibility Support
- SwiftUI provides built-in accessibility features that work automatically with minimal developer effort.
- Unlike UIKit, where you have to manually configure accessibility for each UI element,
  SwiftUI infers accessibility traits, labels, and roles based on the view type and content.
- In short, SwiftUI makes accessibility easier, faster, and more consistent by handling most of it automatically,
  while still allowing customization when needed.
  ### Comparison between UIKit & Swift UI code
 #### UIKit
 ```swift
      let button = UIButton(type: .system)
      button.setTitle("Submit", for: .normal)
      button.isAccessibilityElement = true
      button.accessibilityLabel = "Submit Button"
      button.accessibilityHint = "Submits the form"
      button.accessibilityTraits = .button
```
  #### Swift UI
```swift
      Button("Submit") {
          // Action
      }
      .accessibilityHint("Submits the form")
```
## Qestion 2: What is an Alignment Guide in SwiftUI?
- An alignment guide lets you customize how views line up inside a container like VStack or HStack.
- Normally, SwiftUI aligns views using edges like .leading, .center, or .trailing. But sometimes you want more control right?
  -- like aligning the right edge of one view with the left edge of another.
  ### Example: Align Views to the First Text Baseline
```swift
struct CustomHorizontalAlignmentExample: View {
    var body: some View {
        VStack(alignment: .customCenter) {
            Text("Short")
                .alignmentGuide(.customCenter) { d in
                    d[.leading] + 20  // Offset from leading edge
                }
                .background(Color.yellow)

            Text("Much longer text line")
                .alignmentGuide(.customCenter) { d in
                    d[.leading] + 80
                }
                .background(Color.green)
        }
        .padding()
        .border(Color.blue)
    }
}

// MARK: - Custom Alignment Guide
extension HorizontalAlignment {
    private enum CustomCenterAlignment: AlignmentID {
        static func defaultValue(in d: ViewDimensions) -> CGFloat {
            d[.leading] // Default fallback
        }
    }

    static let customCenter = HorizontalAlignment(CustomCenterAlignment.self)
}

```
