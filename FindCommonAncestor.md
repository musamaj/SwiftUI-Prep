To find the common parent (ancestor) of two UIView instances in a view hierarchy in Swift, you can use the following algorithm:

✅ Algorithm (Concept):
Traverse up the superview chain from the first view and record all its ancestors in a set.

Traverse up the superview chain from the second view, and check if any of its ancestors are in the set of ancestors from step 1.

The first match found is the nearest common ancestor.

🧠 Why this works:
Since each UIView has only one superview, the hierarchy forms a tree, and this approach finds the lowest common ancestor (LCA) efficiently.

```swift
func findCommonAncestor(view1: UIView, view2: UIView) -> UIView? {
    var ancestors = Set<UIView>()
    
    // Step 1: Collect all ancestors of view1
    var current: UIView? = view1
    while let view = current {
        ancestors.insert(view)
        current = view.superview
    }
    
    // Step 2: Traverse ancestors of view2 to find the first common one
    current = view2
    while let view = current {
        if ancestors.contains(view) {
            return view
        }
        current = view.superview
    }
    
    // No common ancestor found
    return nil
}
```
