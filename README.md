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

✅ **What** is an Alignment Guide?  
- An alignment guide in SwiftUI is a tool that lets you customize how views are aligned inside layout containers like **HStack, VStack, or ZStack**.
- Instead of using the default alignment (like **leading, center, or trailin**g), you can define your own alignment point.

✅ **Where** is Alignment Guide used?  
  - It’s used inside layout containers when you want to control how child views line up. You apply it using the .alignmentGuide() modifier on individual views.

   **Example containers:**  
    
    HStack  
    VStack  
    ZStack  
    Custom alignment contexts  
    
✅ **When** should you use it?  

Use alignment guides when:  

Default alignment doesn’t give the layout you want.  
You need precise control over how views align.  
You’re building complex or custom layouts.  
You want to align views based on a specific part (like baseline, trailing edge, etc.)  

✅ **How** does it work?  

You use the **.alignmentGuide()** modifier like this:  
```swift
.alignmentGuide(.leading) { dimensions in
    dimensions[.trailing] // align leading edge to trailing edge
}
```
This tells SwiftUI:  

“Align this view’s leading edge using its trailing edge instead.”  

You can also use math to shift alignment: This moves the alignment point 10 points down from the top.  
```swift
.alignmentGuide(.top) { d in d[.top] + 10 }
```

✅ **Why** is it useful?  

Because it gives you flexibility and precision in layout design. With alignment guides, you can:  

Align views in non-standard ways.  
Create polished, professional UI layouts.  
Handle edge cases where default alignment fails.  


  ### Example:
  ```swift
struct ContentView: View {
    var body: some View {
        VStack(alignment: .leading) {
            Text("A")
                .background(Color.green)

            Text("B")
                .alignmentGuide(.leading) { _ in -20 }
                .background(Color.blue)
        }
        .border(Color.red)
    }
}
  ```
  **🔍 What’s Happening?**  
  
  - VStack(alignment: .leading) normally aligns both texts to the left.  
  
  - We override the second Text("B")'s leading alignment by pulling it 20 points to the left with
    ```swift
    .alignmentGuide(.leading) { _ in -20 }.
    ```  
  
  - You’ll see "B" slightly indented left compared to "A"—like it's nudging out of the border.  

  <img width="300" height="600" alt="Simulator Screen Shot - iPhone 14 Pro - 2025-07-14 at 22 38 05" src="https://github.com/user-attachments/assets/861e38d6-6adf-4a5f-9c92-6f91faeed94f" />  
  


```swift
struct ContentView: View {
    var body: some View {
        VStack(alignment: .leading) {
            ForEach(0..<10) { position in
                Text("Number \(position)")
                    .alignmentGuide(.leading) { _ in
                        Double(position) * -10
                    }
            }
        }
        .background(.red)
        .frame(width: 400, height: 400)
        .background(.blue)
    }
}

```
<img width="283" height="556" alt="Screenshot 2025-07-14 at 9 43 12 PM" src="https://github.com/user-attachments/assets/99851693-bd0f-419f-b76b-e687b34ddd7f" />


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

//MARK:- Store
import ComposableArchitecture

let store = Store(initialState: CounterFeature.State()) {
    CounterFeature()
}

//MARK:- Dependency
import Foundation
import ComposableArchitecture

struct UUIDGenerator {
    var generate: () -> UUID
}

extension DependencyValues {
    var uuidGenerator: UUIDGenerator {
        get { self[UUIDGeneratorKey.self] }
        set { self[UUIDGeneratorKey.self] = newValue }
    }

    private enum UUIDGeneratorKey: DependencyKey {
        static let liveValue = UUIDGenerator {
            UUID()
        }
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
   |- **Dependency** -> Environment has largely been replaced by Dependency in the latest versions of the Swift Composable Architecture (TCA).
   
   ## Explain Reducer 
   
   A reducer is a pure function that  
   |-> Takes current state  
   |-> Takes an action  
   |-> Returns new state  
   
   **Example**: Vending Machine  
   
   State -> (snacks, drinks)  
   Action -> (Select Snacks)  
   Reducer -> (Machine provides your request & provides snacks)  
   
   **Example**: Dependency
   ```swift
@Dependency(\.apiClient) var apiClient
```
  
  If your feature is a game character:

  **State** is your health and score.  
  **Action** is what you do (jump, run).  
  **Dependency** is your tools (sword, shield, map).  
  You don’t carry all tools inside you—you ask for them when needed.  

## Question 5: Does SwiftUI ViewModifiers order affect the results? 

- Yes, The below image showing how the order of SwiftUI ViewModifiers affects the result. 
- In short: 
- .background(Color.red).padding() → background is tight around the text. 
- .padding().background(Color.red) → background wraps the padded area.

## Question 6: What is associatedType and how does SwiftUI use it? 

- AssociatedTypes is a part of the mechanisms for using generics in protocols.
-  SwiftUI view is making heavy use of generics and associatedTypes. 
- SwiftUI uses the associatedType in View protocol, where the body property returns an associatedType of View, meaning View can be any type of View like HStack, ZStack, or ListView.
  
## Question 7: How do you identify performance issues in swiftUI? 

    1. Use Instruments (Xcode Tool) 
    2. SwiftUI Performance Template: Use this in Instruments to track rendering, layout, and view updates. 
    3. Time Profiler: Helps identify slow functions or excessive recomputation. 
    4. Memory Graph: Detect memory leaks or retain cycles. 

## Question 8: How to debug a SwiftUI view? 

	- We use the _printChanges() method to detect changes in a View.
  - Additionally, we can capture the view hierarchy for inspecting the layout or getting more information about the view.

🔍 What Does _printChanges() Do? 

- When you call _printChanges() inside a SwiftUI view, it prints to the console which views are being recomputed due to state changes. This is incredibly useful for: 
- Identifying unnecessary view updates 
- Debugging performance issues 
- Understanding how state affects rendering

## Question 9: How to use gestures in swiftUI? 
- In SwiftUI, gestures are a powerful way to add interactivity to your views. 
- You can use built-in gesture modifiers to detect taps, drags, long presses, magnification (pinch), rotation, and more. Here's a quick overview of how to use gestures in SwiftUI 
  
