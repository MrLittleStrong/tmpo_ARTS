# Swift API Availability

### Links

[Swift API Availability](https://nshipster.com/available/)

### Notes

This article introduced How to solve API availability problem in swift.

Here is some knowledge points：

- use ```@available``` to annotate APIs with available information

- Shorthand specification like, star is a must in the end

  ```swift
  @available(platform version , platform version ..., *)
  
  Example:
  @available(macOS 10.15, iOS 13, watchOS 6, tvOS 13, *)
  ```

- Detailed information

  ```swift
  // With introduced, deprecated, and/or obsoleted
  @available(platform | *
            , introduced: version , deprecated: version , obsoleted: version
            , renamed: "..."
            , message: "...")
  
  // With unavailable
  @available(platform | *, unavailable , renamed: "..." , message: "...")
  ```

  In this way, we can only introduce one platform once in a line. We need to list codes when we need to introduce mutiple platforms to annotate one API.

- ```#available``` can be used to determine whether this API is available.

- ```@available``` can be used to annotate class, function, variables, protocols.... everything.