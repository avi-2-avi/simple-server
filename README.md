# Single threaded server using Rust

**Currently working in the project. IN PROGRESS**

Note regarding simple server.

## Architecture

In the server, the following path will be taken by the incoming requests:

1. TCP Listener: will be useful to listen to the incoming data in bytes.
2. HTTP Parser: the data from the listener needs to be parsed in HTTP.
3. Hander: Will route what needs to be done, executing different code or returning different file depending on the method and path it has. This handler will then create an HTTP response.

4. HTTP Parser (again): will be able to parse from HTTP to what the listener can understand.
5. TCP Listener (again): will send back the response to the client.

## Learning Stuff

### Rust relevant concepts

#### Ownership

Ownership is important in order to understand how data is allocated and deallocated throughout the program. There are three important rules in order to understand ownership. I would suggest watching a vid about it and then reading the <a href="https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html">Ownership chapter</a> of the Rust book since the explanation is great.

The most important part out of it is to see your code and understand if data is allocated. With that, the program can be really efficient.

#### Strings

There's two main types of string in Rust. String slices (the ones with the &) and String type. Understanding this is key to really getting by in Rust. The thing to rememnber here is:

1. They are **not** the same
2. String type is a smart pointer
3. String slice is a pointer to a String type. Like it's name states, a piece of the string (and it's also a pointer, since it has the address to where that piece is and how long it is)
4. String type is mutable (since they are in the Heap memory) and string lice is inmutable.
5. Slicing string slices can be dangerous since they slice the bytes, not the character.

#### Modules

Useful to separate (and organize) the code. Also, to make it reusable.
It's good to know when to use public and private in modules since you want to only access certain stuff. It's kind of similar to C++.
Also, remember that "super" it's used to refer to the parent module when a module is wrapped inside another one.
Additionally, use "use" (no pun intended) in order to refer to a module and to avoid prepending with the whole module path, since it would be annoying. You can use "use" inside of modules in order to shorten this reference path as well.

Every file in Rust is treated as a module. So, in to move your module to a separate file you don't need to wrap it around module, just put its contents. However, in order to use it you have to call is with the "use" keyword.

#### Errors

There's two types of errors: recoverable (can be handled) and unrecoverable (probably a bug). The Result enum is used for recoverable errors and there's two possible states: Ok(T) and Err(E) for succesful and error results respectively.

### HTTP/1.1

It's an application layer 7 protocol which is sent over TCP. In here, the clients exchange individual messages instead of streaming data.

- Requests: Messages send by the client (web brosers usually)

```
GET /search?name=abc HTTP/1.1
Accept: text/html
```

The request starts with a method (GET, PUT, POST...).
After it goes the path, which might contain a query string (followed by a ?)
Then, goes the protocol version (HTTP/1.1)
A new line is added and a list of the requests headers should be passed. These header are in a "key: value" format and are separated by new lines.
After it goes two new lines and then goes the body.

- Responses: Messages send by the server as an answer

```
HTTP/1.1 200 OK
Content-Type: text/html

<!DOCTYPE html>
<html lang="en">
...
</html>
```

The response starts with the protocol and it is followed by the status code (200, 404, ...) and a reason phrase (OK, NOT FOUND, ...).
After a new line it is followed by a list of header.
After two new lines a response body is added. The content can be anything, such as an HTML, Javascript or CSS file; a JSON or any other data format.
