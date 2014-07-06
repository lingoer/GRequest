GRequest
========

An Generic HTTP Request Library

```javascript

Request("https://api.github.com/repos/lingoer/SwiftyJSON/issues").get{
  response in
  
  println(response.headers)
  println(response.MIMEType)
  println(response.statusCode)
  println(response.encoding)
  
  println(response.error)
  
  println(response.content)//HTTP Body as NSData
  println(response.string)//HTTP Body as String
  
  println(response.object)//See Below HTTP Body as Serialized to Custom Object, Default is NSData
  
}


//With GET methods use .query(parameters:Dictionary<String, String>) to pass parameters
Request("https://api.github.com/repos/lingoer/SwiftyJSON/issues").query(["labels":"discuss"]).get{
  response in
}


//Every methods returns an instance of the Request to make functional chainnings
Request("https://api.github.com").path("/repos/lingoer/SwiftyJSON/issues").query(["labels":"discuss"]).get{
  response in
}


//With POST methods use .body(parameters:Dictionary<String, String>) to pass parameters as default
//This will encode body as application/x-www-form-urlencoded
Request("http://www.example.com").body(["key":"value"]).post{
  response in
}


var customBodyData:NSData = NSData()
//You can custom your HTTP Body to POST, with Content-Type provided after it.
Request("http://www.example.com").body(customBodyData, typeString:"application/json; charset=utf-8").post{
  response in
}


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


Request("https://api.github.com/repos/lingoer/SwiftyJSON/issues").query(["labels":"discuss"]).get{
  (response:GResponse<JSONValue>) in
  //If you specify the type of deserializtion to response, object property will be the deserialized Object
  println(response.object)
}


Request("https://www.google.com/images/srpr/logo11w.png").get{
  (response:GResponse<UIImage>) in
  let image:UIImage = response.object!
}


//Request is An " typealias Request = GRequest<NSData> "
//You can specify your default behavior of the Client
let client = GRequest<JSONValue>("http://api.example.com")
client.path("/path/to/resource").get{
  response in
  println(response.object) //Its JSON Now
}


//You can add your custom implementation of your Model deserialization.
//By extention to conform protocol: ResponseDeserialization
extension CustomModel:ResponseDeserialization{
  class func convertFromData(data:NSData!) -> (CustomModel?, NSError?)
}

GRequest<CustomModel>("http://api.example.com").get{
  response in
  let model:CustomModel! = response.object
}
```
