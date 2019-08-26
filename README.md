# Code Challenge

**Lytt**, the first digital assistant that makes it easy for employees to communicate sensitive topics at work and is currently working in a new assistant functionality: recognize a message language and reply with the correct Bot message. Lytt is asking you _to develop this new REST API!_


## 1. What you have to do

Create 3 endpoints that summarize this:

- **Receive**: a text message from a reporting person (user) in any language
    -  then **Detect Language**: German, English and Spanish language only
- **Allow retrieving** a message information, by using its unique identifier
- **Reply/Fetch Automated Replies**: with the correct "Reply" for every user message, in the correct user session and order.
- **Display** the corresponding error messages when information doesn't comply

As this challenge is based on the concept of real-time conversations (chatbots), you are encouraged to use a front-end implementation to demonstrate the challenge (we are big fans of React), **but it's not mandatory**.

In the file `/locales/dialogues.{language}.yml` you can get an insight of how to organize your assistant internationalization functionality.

You are free to explore different solutions and any framework, but you should document and justify your technology choices. **Please write a README explaining your decisions.**


![Imgur](https://i.imgur.com/u58St4X.png)

_An example of conversational interface_


## 2. Receive: a user text message

A user will join a Chat "Session" and can send any message into that session. User messages will always go through this process and messages/session should have an unique, **randomly** generated ID. 

The session is used to keep track of how many messages were sent between a user and the bot and if a refresh occurs, still display past messages _from that session_.
*If the session don't exist you should create it*.


**For security reasons the text of a user message should not be stored, only the metadata of it (language, etc).**

**Request**

    POST "/sessions/{session-id}/messages"
```json5
{
    "text": "{user-text-input}",
    "identifier": "{unique-generated-id}"

}
```

**Response (success)**

```json5
// Supported language
{
  "message": {
    "identifier": "{unique-generated-id}",
    "detected_language": "es",
    "timestamp": "2020-08-01T18:20:00Z"
  }
}
```

**Response (error)**

```json5
// Language is not supported
{
 "error": {
   "code": 422,
   "message": "Unfortunately we don't have support for your language yet."
  }
}
```

_a) Response Status should be 201 for success_
_b) Response Status should be __422__ for error_


## 3. Fetch a message

This endpoint returns the information about a message sent into the session. It can be used to retrieve information about a previous message.

**Request**

```javascript
GET "/sessions/{session-id}/messages/{identifier-id}"
```

**Response (success)**

```json5
    {
    "message": {
        "identifier": "{identifier-id}",
        "detected_language": "es",
        "timestamp": "2020-08-01T18:20:00Z"
    }
}
```

**Response (error)**

```json5
// Message ID doesn't exist for this session
{
  "error": {
    "code": 404,
    "message": "Resource doesn't exist"
  }
}
```

_a) Response status should be 200 for success_
_b) Response status should be 404 for error_
_c) Content-Type should be JSON_



## 4. Reply with Bot Message

The first and second messages sent into the session should define the language that is going to be used in the messages sent by the bot.

This endpoint should provide all automated replies from the bot in the detected language for this session. 
The reason for not including the user messages at the moment is something specific for this challenge.

The replies should be ordered by timestamp in **descending** order.

_After the first message, you should not display the automated "salutation" again_.

**Request**

```javascript
GET "/sessions/{session-id}/replies"
```

**Response**

```json5
{
  "replies": [ // messages should be ordered from newest to oldest
    {
      "message":    "Hallo! Ich bin ein virtueller Assistent und ich bin hier, um zu helfen. Was möchtest du tun?",
      "locale_key": "de.salutation",
      "reply_to":   "{first-message-identifier}",
      "sent_at":    "{timestamp}"
     },
     {
       "message":    "In Zukunft werde ich in der Lage sein, jede deiner Fragen zu beantworten",
       "locale_key": "de.after_salutation",
       "reply_to":   "{second-message-identifier}",
       "sent_at":    "{timestamp}"
     } 
  ]
}
```

_**Hint:**_ You can find language detection libraries for almost any programming language online_

## 5. Optionals

We would love to see an implementation that contains one or more of the items below, **but they are completely optional**:

-   Project uses `Dockerfile` and/or `docker-compose.yml`
-   Chat between user and bot uses `WebSockets`
-   Front-end can be used to send messages
-   Front-end test framework was used
-   Hosting the solution in a cloud provider (https://heroku.com, Amazon AWS, https://now.sh, netlify)


## 6. What we are looking for

For this challenge, we are looking at how you document, test and develop features given the guidelines provided using clean & structured code. We want to see if you are familiar with the concepts of APIs, REST and HTTP. 


## 7. How to deliver this challenge

Please deliver the solution using a public repository URL on GitHub and send it back to your contact person. Your repository should contain full instructions and documentation for us to run and test the project. You can clone and develop on top of this repository as well.


## 8. How we are evaluating

**a. Code Quality**

Is the code clean and understandable? Is the codebase organized in any existing pattern?

**b. Testing**

Is the project properly tested and covering edge cases? If tests are missing, what's the reasoning behind it?

**c. Validation**

Is the API properly validated? Does it validate the data sent to the server?

**d. Technology**

Does the technology options make sense for this project? 

**e. Documentation**

Is the project properly documented with instructions and justifications of the technological choices?
