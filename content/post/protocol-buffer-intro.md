+++
author = "Sanjay Pandey"
title = "Protocol Buffer Basics in Go"
date = "2020-05-16"
description = "It is yet another data format, just like XML or JSON."
tags = [
    "Golang",
]
feature_image = "/images/protocol_buffer.png"
+++

It is yet another data format, just like XML or JSON. It is smaller and faster to transport over wire than JSON or XML. 
<!--more-->
It is widely used for communication protocols and data storage. It is widely being adopted over JSON or XML for being language agnostic, smaller in size and faster to transport. We just have to define the schema and rest is taken care of by protocol buffer. we can use special generated source code to easily write and read structured data to and from a variety of data streams and using a variety of languages.
## How they work ?

We start by writing the schema of data to be serialised in proto files. The protocol buffer message contains name-value pairs. Suppose we want to define schema for Person, it could be defined as -

```
syntax = "proto3";

message Person {
    string name = 1;
    string email = 2;
    string city = 3;
    string state = 4;
    string country = 5;
    int32 age = 6;
    string phone = 7;
}
```
After defining the schema, we compile using the protocol buffer compiler for your application’s language on your .proto file to generate data access classes. If we compile for golang , the data access class will look like

Now we can use this for serialising and deserialising data.
```
person := &pb.Person{}
person.Name = "test"
.... other fields ...
out, err := proto.Marshal(person)
// writing to file
if err != nil {
        log.Fatalln("Failed to encode person:", err)
}
if err := ioutil.WriteFile("phones", out, 0644); err != nil {
        log.Fatalln("Failed to write person:", err)
}
```
```
in, err := ioutil.ReadFile("persons")
if err != nil {
        log.Fatalln("Error reading file:", err)
}
person := &pb.Person{}
if err := proto.Unmarshal(in, person); err != nil {
        log.Fatalln("Failed to parse person list:", err)
}
```

## Why should you use it ?

You could have done above using any other data format easily, lets say if we have to do this without protocol buffer, then in python we can do this as :

```
import json
# open file for storing data

f = open("data.json", w)

# create object
person = {"name": "test", ...}

# store in file
f.write(json.dumps(person))
f.close()
```
```
import json

# open file
f = open("data.json")

# read data
line = f.readline()

#deserialize
data = json.loads(line)
```

If you closely observe, we have done almost same in python, but what makes protocol buffer so special ? . The protocol buffer message size is 3 to 10 times smaller, meaning we can store more efficiently or transport easily over slow networks. They are also 20 to 100 times faster . They are not schemaless unlike json, which makes it less prone to breaking changes over the time. It provides easy language interoperability.

## When should you use it ?

Protocol Buffer offer compelling advantages over other data formats for sending data over wire being smaller in size and faster. They are best suited for internal service communication. We can use them for browser to server communication, but being non readable, they pose debugging and development challenges.


{{< css.inline >}}
<style>
.canon { background: white; width: 100%; height: auto;}
</style>
{{< /css.inline >}}
