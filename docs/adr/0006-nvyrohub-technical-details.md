# 6 - NVyroHub Technical Details

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 10/10/2022, 01:36:27 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

NVyroHub is a lot more complex than our [Custom
Circuit](0005-custom-circuit-technical-details.md). These devices not only
receive incoming messages but it checks these messages against user-defined
input. On the service this is pretty straight forward, in a similar fashion to
our [Custom Circuit](0005-custom-circuit-technical-details.md), we need to
split our UI from the core service.

As this is the case, we will be re-using a large amount of the UI components.
However, we will have a separate UI purely for handling the UX of an NVyroHub
device.

## Decision Drivers <!-- optional -->

* Re-use code
* Takes advantage of our existing skills
* Easy to develop
* Web based
* Requires an API (our UI will talk to this)
* Can be run as a daemon
* Need to store data

## Considered Options

* ESP
* RasperryPi
* Ruby
* C/C++
* VueJS
* Nginx
* MongoDB
* Postgres

## Decision Outcome

Chosen option: "RasperryPi", "Nginx", "MongoDB", "Ruby", and "VueJS", because
this service is quite complex, as in there are a number of parts, it would make
sense to develop this using tools we are _really_ familiar with.

We already know we need to have a web based service, and we've got a number of
years of experience with Ruby. We also have quite a number of years of
experience provisioning production servers, so we can take advantage of this
and create a provisioning tool.

This provisioning tool will allow us to setup, update, and ship to a
"RasperryPi". This will make it easy for us to roll out any new changes.

Along with the custom [UI](https://github.com/boodah-consulting/n-vyro-hub-ui),
we will need to have an [API](https://github.com/boodah-consulting/n-vyro-api)
that it can interact with. Not only is this
[API](https://github.com/boodah-consulting/n-vyro-api) important for
communicating with the
[UI](https://github.com/boodah-consulting/n-vyro-hub-ui), it will be this
[API](https://github.com/boodah-consulting/n-vyro-api), which allows power
users to also interact with their NVyroHub device.

The [API](https://github.com/boodah-consulting/n-vyro-api) will communicate
with the data store layer "MongoDB" and return the data the is being requested
for. As we'll be using "Ruby", for the backend, will use "Sinatra", and
"Grape", to handle our [API](https://github.com/boodah-consulting/n-vyro-api)
functionality.

Both our [API](https://github.com/boodah-consulting/n-vyro-api), and the
[UI](https://github.com/boodah-consulting/n-vyro-hub-ui) will need a web
server, "Nginx" is something we're really comfortable with, and it take very
little setup. We'll use that for serving our users these services.

The other aspect of NVyroHub is the service which actually listens out for
incoming messages and works out whether something needs to be done, or not.

This is done by our [daemon](https://github.com/boodah-consulting/n-vyro-hub).

Our [daemon](https://github.com/boodah-consulting/n-vyro-hub) is also written
in Ruby, but this is a CLI based service, which runs as a
[daemon](https://github.com/boodah-consulting/n-vyro-hub). This allows us to
create a systemd wrapper that can handle the service like any other service.

Finally, we also include our [Messaging Layer](0001-messaging-layer.md), this
is need to allow our devices to send its MQTT messages to NVyroHub. It is
possible for users to setup a remote MQTT service, but as a default we provide
users with an internal version. This is because we do not want to encourage
users to send data outside of their personal network, not unless they know
_exactly_ what they are doing.

As there are a number of dependencies for all of these packages we will need to
[provision](https://github.com/boodah-consulting/n-vyro-build) these services
in a repeatable manner. For this we've decided to go with Ansible. Ansible
isn't as popular as something like Docker, but it is easy to work with. The
jury is still out on the best way of shipping the whole device, but as we lack
this knowledge it makes sense to keep things well documented and simple. This
is why we've gone with Ansible, it's almost self documenting and we can use
this to further automate this process later down the line.

Docker would also introduce yet another layer of abstraction, which would make
it a bit harder to debug our services, if we decide to do so on a RasperryPi.

### Positive Consequences <!-- optional -->

* Web software engineers will be familiar
* Quick development cycle
* Can break up into small service
* One daemon per service
* Can use web development techniques
* Easy to provision devices
* Can provision multiple devices at a time
* Potentially runs on _all_ Linux devices

### Negative Consequences <!-- optional -->

* Performance
* Needs full stack knowledge
* Harder to update remotely
* Need some knowledge of Ansible

## Links <!-- optional -->

* [ADR-0001](0001-messaging-layer.md) - Messaging Layer
* [ADR-0002](0002-message-payload.md) - Message Payload
* [ADR-0004](0004-ui-stack.md) - UI Stack

<!-- markdownlint-disable-file MD013 -->