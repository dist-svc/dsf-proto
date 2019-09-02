# APIs

DSF provides a standard set of APIs available either through interaction with the Daemon, or using delegation.
These APIs may be exercised using the client library or the daemon JSON-RPC over unix socket interface directly, or by using DSF messages via delegation.
Further documentation on API methods is provided as part of the [dsf-core]() implementation.

| Method                                                                     | API      | Description                            |
|----------------------------------------------------------------------------|----------|----------------------------------------|
| create(o: \&Options) -> ServiceHandle                                      | Core     | Create a new service instance          |
| register(s: \&mut ServiceHandle) -> FutureResult<(), Error>                | Service  | Register a service in the database     |
| locate(id: \&Id) -> FutureResult<ServiceHandle, Error<                     | Service  | Locate a service in the database by ID |
| publish(s: \&mut ServiceHandle, d: \&Data) -> FutureResult<(), Error>      | Data     | Publish data using a given service     |
| subscribe(s: \&mut ServiceHandle, o: Options) -> FutureStream<Data, Error> | Data     | Subscribe to the provided service      |
