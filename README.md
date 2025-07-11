# Swift UI - QA
Consist important interview Questions &amp; Answer's

## Qestion 1: Why Swift UI uses struct for view insted of class?
- structs are much better than classes in performance.
- No need to worry about memory leaks & thread saftey.
- Swift UI creates views in multiple times, & using structs helps us in managing these instances effectively.

## Qestion 2:  SwiftUI - Automatic Accessibility Support
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
## Qestion 3: What is an Alignment Guide in SwiftUI?
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


## Qestion 4: What is an TCA (The composable Architecture)?

    - The Swift Composable Architecture (TCA) is a powerful framework developed by Point-Free (Brandon Williams and Stephen Celis).
    - Used for building applications in a consistent, modular, and testable way across all Apple platforms.
   
```swift
import ComposableArchitecture

// MARK: - State
struct AppState: Equatable {
    var count: Int = 0
    var isLoading: Bool = false
    var errorMessage: String? = nil
}

// MARK: - Action
enum AppAction: Equatable {
    case increment
    case decrement
    case fetchData
    case fetchDataSuccess(String) // Example data type
    case fetchDataFailure(String) // Error message
}

// MARK: - Reducer
let appReducer = Reducer<AppState, AppAction, Void> { state, action, _ in
    switch action {
    case .increment:
        state.count += 1
        return .none

    case .decrement:
        state.count -= 1
        return .none

    case .fetchData:
        state.isLoading = true
        state.errorMessage = nil
        return .none // Replace with actual effect if needed
    case .fetchDataSuccess(let data):
        state.isLoading = false
        // Handle data (e.g., update state with it)
        return .none

    case .fetchDataFailure(let error):
        state.isLoading = false
        state.errorMessage = error
        return .none
    }
}
```
   
   **TCA compontent's**
   
   Easy to remember - (The TCA is **SAVER's**) my Architecture
   
   |- **State**       ->  Hold's the App Data  
   |- **Action**      ->  Defines user's & system event's  
   |- **Reducer**     ->  Handles Logic & State mutations  
   |- **Store**       ->  Connects State, action & Reducer  
   |- **View**        ->  Swift UI view connected to the store.  
   |- **Environment** ->  API clients / Analysis.
   
   ## Explain Reducer 
   A reducer is a pure function that  
   |-> Takes current state  
   |-> Takes an action  
   |-> Returns new state  
   **Example**: Vending Machine  
   State -> (snacks, drinks)  
   Action -> (Select Snacks)  
   Reducer -> (Machine provides your request & provides snacks)
  
