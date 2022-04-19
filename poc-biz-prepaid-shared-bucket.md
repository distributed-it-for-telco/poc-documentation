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
  - Customer Device
  - Customer Group (ie. share the same pre-paid service)
  - Service provider (VOD service) 
  - Service vendor

- wamCloud actors

  - service oriented (exposes REST APIs)

    - actor `ServicePlanProvisioner` _used by service vendor_
      - provisionServicePlan(`ServicePlan`) : void
      - `ServicePlan` = [`Service`, `CustomerGroup`, bucket_limit]

    - actor `ServiceUsageAuthorizer` _used by customer_
      - authorize(`ServiceUsage`) : access_key
      - `ServiceUsage` = [`Service`, `Customer`, `CustomerGroup`, bucket_decrement]

  - resource oriented _(kind of)_

    - actor `CustomerGroups`
      - findCustomer(`Customer`) : `CustomerGroup`
      - create() : `CustomerGroup`
      - addCustomer(`CustomerGroup`, `Customer`) : void

    - actor `ServiceBuckets`
      - create(bucket_limit) : `ServiceBucket`
      - consume(`ServiceBucket`, bucket_decrement) : void
      - isEmpty() : boolean

## design choices and decisions

- what storage?
  - k/v store with JetStream vs Redis
  - what characteristics (CAP theorem, eventual consistency, network partition tolerance...)

- identity provider: KeyCloak (as IDP and AS)

- Architectural Design,  [@google.docs](https://docs.google.com/document/d/1JHDyaO7ADll3XT4_SpXqbtzFyrjXTKAqbfI1LwtlQ6E/edit#)

## difficulties & interrogations

- actor design?
  - service oriented vs. resource oriented (as in REST)

- capability providers
  - purely technical, no business?
