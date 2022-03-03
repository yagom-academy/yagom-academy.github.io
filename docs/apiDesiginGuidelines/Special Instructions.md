---
layout: default
title: Special Instructions
parent: API Design Guidelines
nav_order: 4
---

# Special Instructions
- **Label tuple members and name closure parameters** where they appear in your API.
    <details>
    <summary>DETAIL</summary>
    <div markdown="1">

    These names have explanatory power, can be referenced from documentation comments, and provide expressive access to tuple members.

    ```swift
    /// Ensure that we hold uniquely-referenced storage for at least
    /// `requestedCapacity` elements.
    ///
    /// If more storage is needed, `allocate` is called with
    /// `byteCount` equal to the number of maximally-aligned
    /// bytes to allocate.
    ///
    /// - Returns:
    ///   - reallocated: `true` iff a new block of memory
    ///     was allocated.
    ///   - capacityChanged: `true` iff `capacity` was updated.
    mutating func ensureUniqueStorage(
        minimumCapacity requestedCapacity: Int, 
        allocate: (_ byteCount: Int) -> UnsafePointer<Void>
    ) -> (reallocated: Bool, capacityChanged: Bool)
    ```

    Names used for closure parameters should be chosen like `parameter names` for top-level functions. Labels for closure arguments that appear at the call site are not supported.
    

    </div>
    </details>


- **Take extra care with unconstrained polymorphism** (e.g. Any, AnyObject, and unconstrained generic parameters) to avoid ambiguities in overload sets.
    <details>
    <summary>DETAIL</summary>
    <div markdown="1">

    For example, consider this overload set:

    ```swift
    ❌
    struct Array {
        /// Inserts `newElement` at `self.endIndex`.
        public mutating func append(_ newElement: Element)

        /// Inserts the contents of `newElements`, in order, at
        /// `self.endIndex`.
        public mutating func append(_ newElements: S)
            where S.Generator.Element == Element
    }
    ```

    These methods form a semantic family, and the argument types appear at first to be sharply distinct. However, when Element is Any, a single element can have the same type as a sequence of elements.

    ```swift
    ❌
    var values: [Any] = [1, "a"]
    values.append([2, 3, 4]) // [1, "a", [2, 3, 4]] or [1, "a", 2, 3, 4]?
    ```

    To eliminate the ambiguity, name the second overload more explicitly.

    ```swift
    ✅
    struct Array {
        /// Inserts `newElement` at `self.endIndex`.
        public mutating func append(_ newElement: Element)

        /// Inserts the contents of `newElements`, in order, at
        /// `self.endIndex`.
        public mutating func append(contentsOf newElements: S)
            where S.Generator.Element == Element
    }
    ```

    Notice how the new name better matches the documentation comment. In this case, the act of writing the documentation comment actually brought the issue to the API author’s attention.

    </div>
    </details>
