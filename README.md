GRequest
========

An HTTP request library written in Swift.

##Basic Usage
Be simple, as it should be:

```Swift
Request("https://api.github.com/repos/lingoer/SwiftyJSON/issues").get{
  response in
  println(response.content)//HTTP Body as NSData
}
```
If you need more infomation:

```Swift
Request("https://api.github.com/repos/lingoer/SwiftyJSON/issues").get{
  response in
  println(response.headers)
  println(response.MIMEType)
  println(response.statusCode)
  println(response.encoding)
  println(response.error)
  println(response.content)//HTTP Body as NSData
  println(response.string)//HTTP Body as String
  println(response.object)//HTTP Body as Deserialized Custom Object, Default is NSData. See Below for more info
}
```

Use ```.query()``` to pass parameters for GET methods


```Swift
Request("www.example.com/api").query(["labels":"discuss"]).get{
  response in
}
```
As for POST:

```Swift
//This will encode body as application/x-www-form-urlencoded
Request("http://www.example.com").formBody(["key":"value"]).post{
  response in
}
```

```Swift
//This will encode body as application/json
Request("http://www.example.com").jsonBody(["key":"value"]).post{
  response in
}
```
```Swift
//You can custom your HTTP Body to POST, with Content-Type provided after it.
Request("http://www.example.com").body(customBodyData, typeString:"application/json; charset=utf-8").post{
  response in
}
```

More:

```Swift
Request("http://www.example.com").head{
  response in
}
Request("http://www.example.com").put{
  response in
}
Request("http://www.example.com").delete{
  response in
}
Request("http://www.example.com").patch{
  response in
}
```

##Instances And Chainnings

A Request is in fact a ```GRequest<T>```

It's ```typealias Request = GRequest<NSData>``` as default.

And a ```GRequest``` is a Generic class specifiying the behavior of the response.

Most methods returns an instance of the ```GRequest``` to make chains.

For example ```.path()```:

```Swift
Request("https://api.github.com").path("/repos/lingoer/SwiftyJSON/issues").get{
  response in
}

```
###Response Deserialization
As for the response behavior.

It's mostly about Response Deserialization:

```Swift
Request("https://api.github.com/repos/lingoer/SwiftyJSON/issues").query(["labels":"discuss"]).get{
  (response:GResponse<JSONValue>) in
  println(response.object)//It's JSON Now
}
```
***Note for more infomation about the ```JSONValue```, see [SwiftyJSON](https://github.com/lingoer/SwiftyJSON)***

If you need something.Just specify it!

```Swift
Request("https://www.google.com/images/srpr/logo11w.png").get{
  (response:GResponse<UIImage>) in
  let image:UIImage = response.object!
}
```
If you don't want to do that every time:

```Swift
let client = GRequest<JSONValue>("http://api.example.com")
client.path("/path/to/resource").get{
  response in
  println(response.object) //Its JSON Now
}

```
###Extensibility
You can add your custom implementation of your Model deserialization.

By extentions conform protocol: ```ResponseDeserialization```

```Swift
extension CustomModel:ResponseDeserialization{
  class func convertFromData(data:NSData!) -> (CustomModel?, NSError?)
}

GRequest<CustomModel>("http://api.example.com").get{
  response in
  let model:CustomModel! = response.object
}
```
