# Generating Documentation for Extended Types

Generate documentation for the extensions you make to types from other modules.

## Overview

By default, the Swift-DocC plugin ignores extensions you make to types that are not from the module you're generating documentation for.

To include documentation for extended types, add the `--include-extended-types` flag to your invocations:

    $ swift package generate-documentation --include-extended-types

> Note: Extension support is available when using Swift 5.8 or later and the Swift-DocC plugin 1.2 or later.

## Understanding What is Included by Default

Not everything that is declared in an extension is hidden behind the `--include-extended-types` flag. If the extension is declared in the same target as the type it is extending, the extension's contents will be included in the documentation by default.

```swift
public struct Sloth { }

extension Sloth {
    // This function is included in the
    // documentation by default.
    public func wake() { /* ... */ }
}

// `Collection` is from the standard library,
// not the `SlothCreator` library, so this is
// what we call an "extended type".
extension Collection where Element == Sloth {
    // This property is not included in
    // the documentation by default.
    public func wake() {
        for sloth in self {
            sloth.wake()
        }
    }
}
```

## Finding Extended Types in your Documentation

Extended Types are part of the documentation archive of the target that declares the extensions.

![The rendered documentation for SlothCreator/Swift/Collection](extended-type-example)

Within that documentation archive, Extended Types are grouped by the Extended Module they belong to. You can find the latter on your documentation's landing page in a section called "Extended Modules". In our example from above, we have one Extended Module called "Swift" - the name of the standard library. This page can be referenced like this: ` ``SlothCreator/Swift`` `.

The Extended Type ` ``SlothCreator/Swift/Collection`` ` is a child of the Extended Module and is listed under "Extended Protocols" on the ` ``SlothCreator/Swift`` ` page.

> Note: The references above use the full path, including the name of the catalog's target, `SlothCreator`. This should help to understand the symbol's exact location, but usually isn't necessary.

<!-- Copyright (c) 2023 Apple Inc and the Swift Project authors. All Rights Reserved. -->
