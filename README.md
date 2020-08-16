# Networking

This networking libaray can make sending request and decoding response very easy.


## Installation with Swift Package Manager

Once you have your Swift package set up, you add this code to your Package.swift. 
```swift
dependencies: [
    .package(url: "https://github.com/LoniQin/Networking", .upToNextMajor(from: "1.0.0"))
]
```
 In Xcode, you can choose File->Swift Packages->Add Pakcage dependancies, and add https://github.com/LoniQin/Networking.
 
 ## How to use
 The two important protocol are `RequestConvertable`, `ResponseConvertable`. Namely objects and structs that confirms to `RequestConvertable` can convert to URLRequest type, and that confirms to `ResponseConvertable` can convert from URLResponse to that specific type. 
 ### `RequestConvertable` and `ResponseConvertable`
 `RequestConvertable` can convert an object to a `URL`, and `ResponseConvertable` can convert a response Data to types that confirms to it.
 ```
 public protocol RequestConvertable {
     func toURLRequest() throws -> URLRequest
 }
 public protocol ResponseConvertable {
     static func toResponse(with data: Data) throws -> Self
 }

 ```
 ### Types confirms to `RequestConvertable`
 * `String`
 * `URL`
 * `URLRequest`
 * `FormData`
 * `HttpRequest`
 
 ### Types confirms to `ResponseConvertable`
 * `String`
 * `Data`
 * `URLRequest`
 * `JSONCodable`
 
 You can request this README.md using this string that represets a link:
```swift
HttpClient.default.send("https://github.com/LoniQin/Crypto/blob/master/README.md") { (result: Result<Data,Error>) in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let data):
        print(data)
    }
}
```

You can use `URL` and `URLRequest` object to start a http request:
```
HttpClient.default.send(URL(string: "https://github.com/LoniQin/Crypto/blob/master/README.md")!) { (result: Result<Data, Error>) in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let data):
        print(data)
    }
}
HttpClient.default.send(URLRequest(url: URL(string: "https://github.com/LoniQin/Crypto/blob/master/README.md")!)) { (result: Result<Data, Error>) in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let data):
        print(data)
    }
}
```

You may use `HttpRequest` to start a request:
```swift
let request = HttpRequest<None, None>(domain: "https://github.com",
                                      paths: ["LoniQin", "Crypto", "blob", "master", "README.md"])
HttpClient.default.send(request) { (result: Result<String, Error>) in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let data):
        print(data)
    }
}
```

You can define your own type to confirm to `JSONCodable`, to decode JSON data from web server.

```swift
struct User: JSONCodable {
    let name: String
}

HttpClient.default.send("https://raw.githubusercontent.com/LoniQin/Networking/master/Tests/data/mockUser.json") { (result: Result<User, Error>) in
    switch result {
    case .failure(let error):
        print(error)
    case .success(let data):
        print(data)
    }
}
```
**Of course, you can customize your own request and response data by confirming to `RequestConvertable` and `RequestConvertable`!.**
