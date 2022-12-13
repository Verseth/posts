---
created_at: 2022-12-13 12:19:53 +0100
author: Piotr Jurewicz
tags: ['rails event store']
publish: false
---

# Speed up aggregate roots loading with snapshot events

[Rails Event Store](https://railseventstore.org/) 2.7 is coming with a new experimental feature: `AggregateRoot::SnapshotRepository`.

<!-- more -->

In event sourcing, each change to the state of an aggregate root is recorded as an event in the event store, linked to the specific stream.
This allows for a complete history of changes to be tracked but can also lead to slow performance if there are a large number of events for a single aggregate root.

A general rule of thumb is to design short-lived streams regarding the events count. However, you can find yourself in a situation where the streams rapidly grow too big, and loading becomes a performance bottleneck.
It would be best if you had a quick solution to this problem.

This is where Snapshotting comes in.

## Many possible implementations

There are several common patterns for implementing aggregate snapshot features.

They can be treated as a kind of technical event, or they can be stored elsewhere.
If you go with the first approach, you can use aggregate root's stream or a separate one.
It is not so obvious how to dump the aggregate state for persistence
We can also debate whether Aggregate should know about being snapshotted.

So many things to consider made us blocked from providing a ready-to-use solution for some time.
During our last RES camp in Poznań, we decided to break the deadlock and offer a simple solution to check if it is a good fit for the community.

## RES implementation

For an initial implementation, we decided to go with an alternative aggregate root repository storing snapshots in a separate stream for a given interval of events.

```ruby
# save aggregate snapshot on each 50 events
repository = AggregateRoot::SnapshotRepository.new(event_store, 50)
# or stick to the default interval of 100 events
repository = AggregateRoot::SnapshotRepository.new(event_store)
```

<img src="<%= src_original("speed-up-aggregate-roots-loading-with-snapshot-events/snapshotting-transparent.png") %>" width="100%">

In the above example, using snapshots every 100 events, with aggregate root `Order` having 202 events, we would have only 2 events to read from the event store to load the aggregate root.
* At first, we check for the latest snapshot event in the `Ordering::Order$bb7c6c8b..._snapshots` stream. We initialize the aggregate root state from the event data if it exists.
* Then, we read the remaining domain events from the `Ordering::Order$bb7c6c8b...` stream and apply them to the aggregate root one by one.

A standard implementation would go through all 202 domain events to achieve the same result. STONKS!

### Dumping the state
Under the hood, we use Ruby's `Marshal` library to dump the aggregate root state into a byte stream, allowing them to be stored in the event data.
Marshal format has its limitations. Dumping an aggregate root will not work if its instance variables are bindings, procedure or method objects, instances of class IO, or singleton objects.
It is also impossible to load a dump created with a different major version of `Marshal`.

`AggregateRoot::SnapshotRepository` deals with these possible problems by catching errors and falling back to the standard implementation of linear reading of all the events from the aggregate roots stream.
To ensure whether aggregate roots are dumped and restored with the snapshot support, you can subscribe to a specific `ActiveSupport::Notifications` using `AggregateRoot::InstrumentedRepository`.
```ruby
require 'logger'
instrumented_repository = InstrumentedRepository.new(repository, ActiveSupport::Notifications)
logger = Logger.new(STDOUT)

ActiveSupport::Notifications.subscribe("error_occured.repository.aggregate_root") do |_name, _start, _finish, _id, payload|
     logger.warn(payload[:exception_object].message)
end
```