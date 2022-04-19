# Shared pre-paid service

## concepts to be proofed

- techno hands-on
  - tool stack
  - rust development
  - interface definition w/ Smithy

- data persistance
  - via built-in Key-Value JetStream service
  - via external datastore (Redis, SQL...)

## use-case

- P0 a vendor sells a pre-paid service that can be shared among different users
  - for instance: 20 films on a VOD platform, 100 Go of mobile internet connection
  - the system must deny the usage of the service when the shared-bucket is empty

- P1 users are identified/authenticated

## system design

- system actors _(not wasmCloud actors)_
  - Customer
  - Customer Devices
  - Customer Group (ie. share the same pre-paid service)
  - Service provider
  - Service vendor

- wamCloud actors
  - service oriented

    - `ServiceAuthorizer`
      - provisionService(`Service`, `CustomerGroup`, bucket_limit)
      - authorize(`Service`, `Customer`, bucket_decrement)

  - resource oriented _(kind of)_

    - `CustomerGroups`
      - findCustomer(`Customer`)
      - create()
      - _instance_.addCustomer(`Customer`)

    - `ServiceBuckets`
      - create(bucket_limit)
      - _instance_.consume(bucket_decrement)
      - _instance_.IsEmpty()

## design choices and decisions

- what storage?
  - k/v store with JetStream vs Redis
  - what characteristics (CAP theorem, eventual consistency, network partition tolerance...)

- identity provider: KeyCloak (as IDP and AS)

## difficulties & interrogations

- actor design?
  - service oriented vs. resource oriented (as in REST)

- capability providers
  - purely technical, no business?
