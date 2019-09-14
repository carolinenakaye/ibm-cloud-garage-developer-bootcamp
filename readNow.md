## Fetching Data With URLSession

1. Set up the HTTP request with URLSession

```swift
let session = URLSession.shared
let urlString = https://flight-engine.com/flights?date=2019-09-13&origin=lax&destination=dfw
let url = URL(string: urlString)!
```
	
2. Make the request with URLSessionDataTask

```swift
let task = session.dataTask(with: url, completionHandler: { data, response, error in

    // Do something...
})

task.resume()
```

3. Quickly print the returned response data

```swift
let task = session.dataTask(with: url) { data, response, error in
    print(data)
    print(response)
    print(error)
}
```

4. Properly validate the response data

```swift
if error != nil {
    // OH NO! An error occurred...   
    self.handleClientError(error)
    return
}
```
	Let’s check if the HTTP status code is OK. 

```swift
guard let httpResponse = response as? HTTPURLResponse,
      (200...299).contains(httpResponse.statusCode) else {
    self.handleServerError(response)
    return
}
```

5. Convert the response data to JSON

```swift
do {
    let json = try JSONSerialization.jsonObject(with: data!, options: [])
    print(json)
} catch {
    print("JSON error: \(error.localizedDescription)")
}
```
