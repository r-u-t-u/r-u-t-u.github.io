---
title: Advantages of GraphQL APIs over RESTful APIs
tags: 
 - graphql
 - rest api
---

GraphQL is a query language to APIs. Nowadays it is getting lot of hype in developer's community. GraphQL was initially developed at Facebook and after it was open sourced in 2015, it is widely adopted by tech companies like Twitter, Github and many more.

#### <b>Complexity of integrating REST APIs</b>

Today most of the web APIs are microservices based or has service oriented architecture and are RESTful services. If we think of RESTful APIs, it is like CRUD operations to resources. Now suppose we are developing a facebook home page. We need to get data like notifications, user feeds, ads, what's trending, friend stories when user logins in and display on homepage. So there would be APIs something similar to 
```javascript
GET {base-url}/user/{fb-id}/profileDetails
GET {base-url}/user/{fb-id}/feeds
GET {base-url}/user/{fb-id}/advertisements
GET {base-url}/user/{fb-id}/newNotifications
GET {base-url}/user/{fb-id}/trending
GET {base-url}/user/{fb-id}/friendSuggestions
```
This would require making 6 RESTful services call along with the code complexity in UI to orchestrate these multiple calls and manipulate the data as per the needs on UI.

<div class="row">
    <div class="col-sm-3"></div>
    <div class="col-sm-6">
        <img src = "{{ site.baseurl }}/theme/img/front-and-backend.jpg"/>
    </div>
    <div class="col-sm-3"></div>
</div>

<br/>
With GraphQL, we have to sent the query like below which will reduce to one http call and get only the required data. Instead of UI directly calling these services, it can talk to GraphQL server where GraphQL resolvers take care of services integration. This can greatly improve the performance as there is only one call and compared to client-server latency, server-server latency is negligible. GraphQL queries can be used for data-fetching, while to update, delete data `mutations` are used. It is independent of the technology and can be integrated in the Javascript framework(Angular, React, Vue) or Mobile apps (android, ios) in frontend and any backend services or database.  
```javascript
{
    query(fb-id) {
        profileDetails {
            name
        }
        feeds {
            content {...}
            comments {...}
            likes {...}
        }
        newNotifications {...}
        advertisements {...}
        trending {...}
        friendSuggestions {...}
    }
}
```
This is very declarative and we get only what we ask for. This could be very productive for UI developers. Graphql provide Graphiql tool in browser where we can play with the queries as defined in GraphQL schema. As GraphQL is domain driven design it could be also useful to Product managers to design products over services and collaborate easily with technology.

#### <b>GraphQL as BFF(Backend For Frontend)</b>

<div class="row">
    <div class="col-sm-1"></div>
    <div class="col-sm-10">
        <img src = "{{ site.baseurl }}/theme/img/data-graph.jpg"/>
    </div>
    <div class="col-sm-1"></div>
</div>
<br/>
Often there are many client web and mobile applications who uses the same APIs. Every client application has different need and requirements. RESTApis are basically CRUD operations to resources. If we design RESTApis as atomic it results in making multiple calls to server. If we have any new requirements for one client front-end application to add new fields, instead of making new API we add it in the existing API. This results in overfetching of data for other clients especially for mobile apps. So no matter how perfectly we design these APIs, it is not perfect for all the clients as they are RESTful services.
Here GraphQL act as a client adapter and clients can query data fields specific to them. It acts like BFF (Backend For Frontend).  

If we look at the history of web, GraphQL seems to be better option for todays modern web and mobile applications. Web first went live in year 1992 and consisted of HTML, HTTP, CSS. To generate dynamic responses we had server side languages like PHP(scripting language - LAMP stack), Java and JSP, ASP.NET. In midst of all javascript was web scripting language for interactive experience with browser and DOM. Then Jquery and XMLHttpRequest aka AJAX was used to load data in background. With advent of mobile and Single Page Applications,the data is consumed asynchronously in background from REST APIs. However complexity of applications increases as more and more APIs are introduced. GraphQL can help greatly to reduce this complexity of multiple API integrations and manipulation.

#### <b>API versioning and depreciation</b>

Adding new fields to existing APIs is easy but changing the format of response data or removing existing fields is difficult. GraphQL has directives like @deprecated, @rename which can be added to type field levels. Using these directives API versioning and depreciation can be easily handled.

```ruby
type Car {
  id: ID!
  make: String
  model: String
  description: String @deprecated(reason: "Field is deprecated!")
}
```

#### <b>GraphQL Schema Definition Language and Strong Type System</b>

Web services are way to communicate between applications. Mainly SOAP or REST based services are used. SOAP are XML based and is a standard protocol unlike REST services. The amount of data exchanged in SOAP based service is huge and hence REST services is considered as fast and light-weight. However there are still SOAP services used today for security concerns. In soap based services there are WSDL(Web service description language) documents which describes web service and methods it exposes along with params and types. In similar way GraphQL schemas acts like WSDL.  

GraphQL gives strong type system expressed using syntax called GraphQL Schema Definition language. Hence a query written in client can be verified before it is run or committed in code base. A change in server schema like type change can also be checked. 

Another great thing about GraphQL APIs is that documentation are automatically generated and always upto date.

#### <b>Subscriptions for real time functionalities</b>

If we want to implement real time functionalities like notifications, showing live cricket scores, charts with real time changing data, there are different ways to do it. Polling of the data is normal approach but it is very inefficient way. We can also use Firebase Realtime database, libraries like [Socket.io](https://socket.io/). Http calls are stateless and to have duplex data following between client and server websockets are used in such libraries. There also message queue, JMS to handle messaging. Apache Kafka is also now popularly used to stream real time data. The UI needs to handle this streaming data using Observables.

Apart from queries and mutations, GraphQL also gives Subscriptions where we can subscribe to real time data. It is like publish-subscribe model. Subsciptions work on top of websockets. Hence implementing real time functionalities can be easy with GraphQL.

[comment]: <> (#### Cons of GraphQL)
[comment]: <> (Nested queries and Dataloaders)

[comment]: <> (Single point of failure and use of schema stiching)

#### <b>Getting started with GraphQL</b>

If you want to explore and play with such GraphQL queries. 
There is Twitter's GraphQL Playground -  [Twitter GraphQL Hub](https://www.graphqlhub.com/playground/twitter)

Github API v3 is REST based, while in v4 they have transitioned to GraphQL - [Github API v4](https://developer.github.com/v4/)<br>
Check there GraphQL explorer to play with such queries -
[Github API v4 explorer](https://developer.github.com/v4/explorer/)

<img src = "{{ site.baseurl }}/theme/img/github-graphql.jpg" height="350" width="700"/>

If you wish to learn more about graphQL you can start with [GraphQL](https://graphql.org/). Happy Learning!!
