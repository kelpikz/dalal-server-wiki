Welcome to the Dalal Street Server wiki!

## Technologies Used

### gRPC:

gRPC is a RPC API architechture written by Google. The reason we use gRPC instead of any normal RESTful API architecture is because our game should be able to handle large number of requests per second. RPC APIs offers very less payload (in requests) compared to RESTful APIs. gRPC also offers bidirectional streaming events which we use for various purposes like stock price updates , sending notifications etc. gRPC natively uses HTTP 2/ which offers many advantages than HTTP 1.1/ like multiplexing over single connection, server push , better compression of network requests which decreases our payload to a great extent.

### Golang :

Golang is the programming language used to write the server gRPC methods.

### Protocol Buffers:

Protocol buffers is a programming language independent mechanism. gRPC uses this for communication (similar to JSON , XML). This also adds upto the efficiency of our server by serializing the data efficiently.

## Game Workflow

- All grpc requests happen over a http2 **\_\_\_**.
- So in order to communicate with the server, the client establishes a **HTTP2** connection with the server.
- Once the connection is established and successfully logs in by calling appropriate methods
- The user can subscribe to various streams including market depth, stock prices, stock histroy etc. You can find more about the streams [here](https://github.com/delta/dalal-street-server/wiki/Server-Docs#streams-).
-

## Features

- A quick walk through on all the implemented features and how they are implemented

#### 1. Orders

A user can make 3 types of transactions, in each type the user can either place a **ask** or a **bid** order.

1. **Market order** -
2. **Limit order** -
3. **Stop-loss order** -

All the above 3 orders can either be

1. **Bids order** - Users _bids_ for given stock
2. **Ask order** - Users _ask_ some money for given stock

Before a order is approved and added to the [order book](order_book_link), a series of checks are made including:

1. Check if the company is bankrupt.
   - If the company is bankrupt, its stocks become worthless.
2. The order size should be less than the `BID_LIMIT` / `ASK_LIMIT`.
   - This is the maximum number of stocks for which you can place a single order. This can be found [here](https://github.com/delta/dalal-street-server/blob/master/models/Constants.go).
   - This is done to ensure. (I have no idea @abekshek pls help)
3. There are some special checks for **short selling**.
   - If the user is short-selling a stock, the stocks left after short selling must be greater than `SHORT_SELL_BORROW_LIMIT`.
   - We check if the user's net worth is more than short selling stock's worth. This is a extra constrain to deter users from short selling when they dont have enough cash to repay it. This check is done to induce a sense of parallelism betwween real world short selling and our game.
4. The user should have enough cash / stocks to place the order.
   - _Suprised pikachu face_ !!
5. the price should be inside `ORDER_PRICE_WINDOW` _( + or - 20% of the current stock price )_

After these checks are successfully completed, the order price and a order fee (3% of order fee / askWorth) is deducted from the user the stocks part of the ask order is moved to reserved stock.

#### 2. Exchange

- Users can directly buy stocks from the exchange provided they have the money to complete the transaction and the total stocks bought is less than the `BUY_LIMIT`.

#### 3. Mortgage

- Users can mortgage the stocks they own for extra cash to spend in the market.

> Should the doc be more technical or should it be more descriptive ??

## Streams

<!-- # TODO  -->

### How a client can subscribe to a stream

- After a user logs in, they can make a request to [`dalalStreamsService.Subscribe`](https://github.com/delta/dalal-street-server/blob/2348bab37b9da2947cc378440fe24aa9a5140dc2/grpcapi/streamservice/DalalStreamService.go#L117), with your sessionToken and [stream_id](https://github.com/delta/dalal-street-proto/blob/26198dfdb906b89d0caeebb3f034613204afc012/datastreams/Subscribe.proto#L7). You will receive a `subscription_token` as a response.
- With this new `subscription_token`, you can make a request to the corresponding Stream method along with the sessionToken.
<!-- TODO: More about session token ??????????  -->

The available streams include :

1. Transactions Stream
2. Notification Stream
3. Stock Price Stream
4. Stock Exchange Stream
5. **Market Depth Stream** - Stream that emits new orders and completed transactions for given stock.
6. **Market Events Stream** - Streams of market events (aka news)
7. My Orders Stream
8. Stock History Stream
9. Game state Stream

## Determining Stock Price

## Concurrency model

## Matching Engine and Order book

## Handling Race Conditions
