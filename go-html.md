# http in GO tutorial
Go's standard library provides robust, built-in support for both creating HTTP servers and making HTTP requests as a client via the net/http package. 

In python,python ask entire response body and put it in r.text
in go lang `http.Get()` only return headers but connection keeps open
The server has already sent (or is sending) the entire response over the TCP	connection
and the data is kernel network buffers waiting to be read.

```
response,error := http.Get("https://api.restful-api.dev/objects/4")
	if error != nil{
			panic(error)
		}
```

must run as it Schedules response.Body.Close() to run when main() exits
`defer response.Body.Close()`

`defer` refers to even if the code crash still execute it

it realeases the file descriptor so the go can reuse the tcp file descriptor
connection to be returned to the connection pool for reuse (HTTP keep-alive).
can also put response.Body.Close() at the end but connection may still be open if it crashes


**reading response as text**

`io.ReadAll(resp.Body)`
when you run this
This drains data from the kernel's network
buffers (where the server's response is already sitting) into our
program's memory as a []byte slice. No additional request is made to
the server - we're just consuming data that was already transmitted.

```
b, err := io.ReadAll(response.Body)
    if err != nil {
        panic(err)
    }
fmt.Println(string(b))
```

**reading response as json**

"encoding/json" import is required to get all json funtionality

first we have to make a struct to store the json data in format its not like python which gives automatically its for men >:)

```
type PhoneRes struct {
    ID   string `json:"id"`
    Name string `json:"name"`
    Data Dat    `json:"data"`
}

type Dat struct {
    Price float64 `json:"price"` // number in JSON → float64 in Go
    Color string  `json:"color"`
}
```

this is for `{"id":"4","name":"Apple iPhone 11, 64GB","data":{"price":389.99,"color":"Purple"}}`


now we have to convert response.Body into this struct for doing that we do

`json.NewDecoder(response.Body)` -> json.NewDecoder() build upon io.ReadAll which drain the from buffers and then convert it into a struct

*note:- if you run io.ReadAll(response.Body) before this it will drain the data and it will raise a error*

to read from already drained data we use:

`json.Unmarshal(data, &struct)` data is the `[]bytes`  this reads from existing bytes and convert it into struct

all in action:
```
package main

import (
	"fmt"
	"io"
	"net/http"
	"encoding/json"
)


type PhoneRes struct {
    ID   string `json:"id"`
    Name string `json:"name"`
    Data Dat    `json:"data"`
}

type Dat struct {
    Price float64 `json:"price"` // number in JSON → float64 in Go
    Color string  `json:"color"`
}


func main(){

	response,error := http.Get("https://api.restful-api.dev/objects/4")

	if error != nil{
			panic(error)
		}
	defer response.Body.Close() 

	b, err := io.ReadAll(response.Body)
    if err != nil {
        panic(err)
    }
   	fmt.Println(string(b))

   	var phone PhoneRes 

   	json.NewDecoder() 
	decoder := json.NewDecoder(response.Body)
	if err := decoder.Decode(&phone); err != nil {
	    fmt.Println("error decoding response body")
	    return
	}

	fmt.Println(phone.Data.Price)


	derr := json.Unmarshal(b, &phone);
	if derr != nil {
		fmt.Println("error decoding response body")
	    return
	}
	fmt.Println(phone.Data.Price)
	fmt.Println(response.Status)
	fmt.Println(response.Header)

}
```

**creating a request (GET)**

we only looked at http.Get() but hold up!

we also have `http.NewRequest(method,url,body)` this give us more control to request. this create a request object

*note:- it dosent open socket send request or anything we have to make http.Client for that*

u can use `req.Header.Set("User-Agent", "my-go-client/1.0")` to set headers a

```
client := http.Client{}
response, err := client.Do(req)
if err != nil {
	fmt.Println("error making request: ", err)
	return
}
defer response.Body.close()
```

now make a client to actually send the request 

```
	request,error := http.NewRequest("GET","https://google.com",nil)

	request.Header.Set("User-Agent", "my-go-client/1.0")

	if error != nil{
		fmt.Println("error bro")
		return
	}

	client := &http.Client{}
	resp, err := client.Do(request)
	if err != nil{
		fmt.Println("error bro 2")
		return
	}
	defer resp.Body.Close()

	b, err := io.ReadAll(resp.Body)
    if err != nil {
        panic(err)
    }
   	fmt.Println(string(b))

```

**creating a request (POST)**

main difference between get and post is post has body or payload which we send.
that can we crafted with: 
`json.Marshal(Struct)`

like in get request we filled the struct with data
now we convert that struct into a json

first create a json struct
```
type User struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}
```

now convert it into json

```
u := User{Name: "Alice", Age: 20}
    
    body, err := json.Marshal(u)
    if err != nil {
        log.Fatal(err)
    }
```

now send the rocket `bytes.NewReader` convert into buffer (bytes)

`req, err := http.NewRequest("POST", "https://httpbin.org/post", bytes.NewReader(body))`


full code:

```
    u := User{Name: "Alice", Age: 20}
    
    body, err := json.Marshal(u)
    if err != nil {
        log.Fatal(err)
    }

    req, err := http.NewRequest("POST", "https://httpbin.org/post", bytes.NewReader(body))
    if err != nil {
        log.Fatal(err)
    }

    req.Header.Set("Content-Type", "application/json")
    req.Header.Set("Accept", "application/json")

    client := &http.Client{Timeout: 10 * time.Second}
    resp, err := client.Do(req)
    if err != nil {
        log.Fatal(err)
    }
    defer resp.Body.Close()

    log.Println("Status:", resp.Status)
```


