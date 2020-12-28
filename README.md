# Messaging-options-in-Azure
Messaging options in the Microsoft Azure Platform

Messaging versus Event
Messages and events are not the same. Messages have a clear intent — they are sent for a specific action or response. When a message is sent to a warehouse system requesting an item to pick — this message has intent or a command that requires an action — the client or system sending the message is expecting a response in this case. An event, on the other hand, reflects a fact — an occurrence in the past. Let’s say the requested item leaves the warehouse and the system sent out a notification message there could be many systems or devices interested in it. This notification, i.e., event, however, doesn’t define intent as the publisher is not aware of these systems and devices nor doesn’t have to be.
![image](https://user-images.githubusercontent.com/58148717/103222943-cd5e7980-48ea-11eb-961c-9596f5510860.png)

Azure Service Bus
The Service Bus is one of the oldest services of the Azure platform — it is a highly reliable, brokered cloud messaging system. This service is aimed at enterprise messaging scenarios and offers middleware technologies like message queueing and publish/subscribe messaging. Furthermore, the service bus provides decoupling between services and applications. The critical capabilities of the service bus are

	• Queues are offering an asynchronous messaging capability with first-in-first-out (FIFO) message delivery. Only one consumer receives each message. Messages are persisted in the queue, and the consumer does not have to pick up the message straight away, i.e., the message publisher and consumer do not have to be connected to queue at the same time. Hence, queues are suitable for store-and-forward messages in a particular scenario.
	• Topics and subscriptions offer a similar capability as queues. However, there can be more consumers for messages that are sent to a topic. A topic has one or more subscriptions. Furthermore, subscriptions can have filters, and thus various messages to a topic can end up in different subscriptions belonging to the topic.
	• Relay provides a gateway to connect on-premise services to Azure, without having to open a firewall connection to the network. 
	
	• The Relay has two features — Hybrid Connections and WCF-Relays. Both enable a secure connection to on-premise services. The difference is that the hybrid connection uses the open standard web socket, while WCF-Relay uses the legacy Windows Communication Foundation (WCF) to enable remote procedure calls.

    Azure Service Bus is available in three tiers:
	• Basic — queues and scheduled messages, and message size up to 256 KB.
	• Standard — on top of basic with Topics and subscriptions, transactions, sessions, and de-duplication.
	• Premium — on top of the standard, and message size up to 1 MB.
	
	Service Bus in Action (Enterprise Messaging in Azure)
	With the Azure messaging in Service Bus, there are several scenarios you can think of where this service brings value. Service Bus offers, topics and subscriptions (pub-sub), and message queues. When you require a message broker in Azure, then Service Bus is fit for your solution.
	
Assume you want to process flat-file batch files (1,2) in Azure through multiple micro-services — each service has one responsibility. The microservices (3) shown in the diagram below are activated by a message in a service bus queue. The queue is an intermediary between services and decouples them from each other. The microservices are packaged into a container and are hosted in a managed Azure Kubernetes Cluster (AKS). The microservices can scale, and this also accounts for the queues, i.e. they can handle the massive throughput of messages that can activate the microservices to process the EDI files (parse, translate, store) (4).
	For more examples visit link below
	https://github.com/Azure/azure-service-bus/tree/master/samples

![image](https://user-images.githubusercontent.com/58148717/103223004-f121bf80-48ea-11eb-90b1-e37c19f11a6f.png)

Azure Event Grid
Event Grid is one of the latest services added to the Azure Platform. This service enables event-driven, reactive programming through a publish-subscribe model With Event Grid, there is a concept of publishers of events, which do not expect how the events are handled, and subscribers who decide how to handle the events. Publishers emit events to Event Grid — centrally manages events by routing them to subscribers of them. Furthermore, the routing can be done on the event type, or by a pre- and or postfix.
Event Grid has the following characteristics
	• dynamically scalable
	• low cost
	• serverless
Both Azure Service Bus with Topics and Subscriptions and Event Grid offers a pub-sub model. However, they are different in behavior as shown in the picture below.
![image](https://user-images.githubusercontent.com/58148717/103223066-11517e80-48eb-11eb-88b0-fc2f1a2219ca.png)

Event Grid in action
Handle events with Event Grid
The Event Grid service on the Azure Platform is suitable to route events coming from Azure resources like Event Hub, Azure Storage, and third party (custom) to any given event handler or WebHook. You can, for instance, create an Event Grid Topic, and send your application events to Event Grid to leverage its reliable delivery, advanced routing, and direct integration with Azure. Furthermore, you can make use of Event Grid and Logic Apps to process data without writing code.
Let’s assume you ingest public data, i.e. weather data using a Logic App with a schedule trigger mechanism. The Logic App processes an array of data and stores every part of the array as an individual JSON file in blob storage. The blob storage is configured with Event Grid, and one or more Logic Apps handles each blob created event.
![image](https://user-images.githubusercontent.com/58148717/103223105-29290280-48eb-11eb-9c95-afb421f9df2c.png)

Azure Event Hubs
You can view Azure Event hub as a significant data pipeline. This service is designed to ingest large data streams generated by devices and services. Moreover, Event Hubs facilitates the capture, retention, and replay of telemetry. In case your solution requires a high throughput of data ingestion then Event Hubs is the right service.
With Event Hubs, you can define so-called ‘consumer groups’, which allows you to read the stream of events. In case you have one receiver read the stream of events, then you can use the default consumer group. However, when you have multiple receivers for the stream concurrently, and each wants to read the stream at its rate — then you will need to set up numerous consumer groups. Furthermore, each receiver will manage an index (or offset) — a pointer to where in the stream of events it will start reading. The receiver can begin at the beginning of the stream and read to the end and subsequently wait for new events — or it can start reading partway through the stream.
![image](https://user-images.githubusercontent.com/58148717/103223160-3f36c300-48eb-11eb-9b7e-cf49832e39ee.png)

Storage Queues
Like Service Bus Queues, Azure storage queue offers asynchronous messaging, and the publisher and consumer do not have to be connected at the same time. The message can be stored and picked up later by the consumer. A storage queue is suitable as a task queue, and messages can remain in the queue for a maximum of seven days.
Azure Storage Queues differ from Service Bus Queues — each serves a different purpose (see picture below).
![image](https://user-images.githubusercontent.com/58148717/103223205-52499300-48eb-11eb-9a3b-b48cf349099f.png)

Storage Queue in action. 
Task Queue
Storage Queues are a part of the Azure Storage Account service. Furthermore, they feature a simple REST-based GET/PUT/PEEK interface, providing reliable, persistent messaging within and between services. You can consider storage queues:
	• when you must store a significant amount of message, i.e. over 80 GB,
	• time-to-live for your messages is less than seven days,
	• your application needs to track progress for processing a message inside of the queue,
	• or require server-side logs of all of the transactions executed against your queues.
When for instance, you want to process a workload asynchronous or pass messages from one web role to a worker role or function, you can use a simple queue like Storage Queue. Similar to the sample in the Enterprise Messaging scenario you can use a Storage queue, however, if it needs to be in an ordered manner, then Service Bus queues are a better fit because of the strict ordering (First-in-first-out).
Assume you need to upload a large number of high-resolution images and each need to be resized and analyzed asynchronously — then images are stored first, and then task messages are sent to storage queues, i.e., task messages for resizing on one storage queue, and task messages for analysis on another.
![image](https://user-images.githubusercontent.com/58148717/103223257-71e0bb80-48eb-11eb-9544-11250084193e.png)

Below is the summary of these services.

![image](https://user-images.githubusercontent.com/58148717/103223288-81f89b00-48eb-11eb-9e12-c5396e00d859.png)















