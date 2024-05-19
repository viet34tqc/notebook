# Webhooks

Are a way for an app to send real time data to another app, typically triggered by an event (ex: sending a message). When an event is triggered, the webhook send a POST request to other application with relevant data.

Compare with API: 

- API is also a way for 2 applications to communicate with each other. However, API is request driven, that means 2 application communicate when client sends the request and server responses
- Webhooks is event-driven, clients don't need to send the request to server, server will automatically send payload as a POST request to client’s webhook URL when specified event occurs

## Examples

- A person sends a message in a group, all the members in that groups receive the notification
- When we push a commit to github, github sends an email to our address

## Structure of webhooks

- Event trigger: The initial action or event in the source application (server)
- Payload data: the data associated with the event
- Callback URL: an endpoint in target application to receive data (client)
- POST request: payload is sent as a POST request to callback URL. This happens when the event is triggered
- Listener at Callback URL: The handler at the target application that listens for the incoming POST request
- Data processing: The target app processes the data and send the response
