Welcome to the Dalal Street Server wiki!

## Technologies Used

### gRPC: 
gRPC is a RPC API architechture written by Google. The reason we use gRPC instead of any normal RESTful API architecture is because our game should be able to handle large number of requests per second. RPC APIs offers very less payload (in requests) compared to RESTful APIs. gRPC also offers bidirectional streaming events which we use for various purposes like stock price updates , sending notifications etc. gRPC natively uses HTTP 2/ which offers many advantages than HTTP 1.1/ like multiplexing over single connection, server push , better compression of network requests which decreases our payload to a great extent.

### Golang : 
Golang is the programming language used to write the server gRPC methods. 

### Protocol Buffers: 
Protocol buffers is a programming language independent mechanism. gRPC uses this for communication (similar to JSON , XML). This also adds upto the efficiency of our server by serializing the data efficiently.