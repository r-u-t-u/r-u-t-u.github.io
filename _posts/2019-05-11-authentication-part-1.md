---
title: Authentication[Part 1] - Session-based authentication
tags: 
- http-session
- java
- spring
- rest api
---

If we are making web applications using J2EE in servlets or jsp pages, (or even Spring MVC framework which uses DispatcherServlet and jsp pages) we need to remember the conversational state of clients.
As HTTP is stateless, we need to handle sessions to check all the requests coming from same client from login to logout or till session expires. Session Management is old way to track user activities and used in traditional web apps where all pages are server rendered.

<!-- . However with emergence of REST Apis every request is authenticated by accesstokens and it actually requires making application stateless. -->

##### <b>HTTP Sessions</b>

To enable session tracking in Spring MVC we need to use Spring security and its dependency need to be added to pom.xml if maven is used as build tool. 

1. In web.xml, the deployment descriptor we have to configure DispatcherServlet, servlet mappings, and security filters. The session related        configurations<br> 
    `session-timeout` element is in minutes for session timeout <br>`http-only`  insures that only server can access the cookie<br> `secure` cookie can only be sent on HTTPS

    ```xml
    <session-config>
            <session-timeout>10</session-timeout>
            <cookie-config>
                <http-only>true</http-only>
                <secure>true</secure>
            </cookie-config>
    </session-config>
    ```

2. Spring Configurations <br>
    Via `create-session` attribute we can control when session are created. Similary we can configure concurrent sessions, session timeouts. We can also set spring configurations by Java code and using annotation `@EnableWebSecurity` instead of xml configurations.

    ```xml
    <http create-session="always" >
        ...
        <session-management invalid-session-url="/invalidSession.html">
            <concurrency-control max-sessions="5" expired-url="/sessionExpired.html"/>
        </session-management>
    </http>
    ```
    When user enters credentials we need to authenticate it. The Spring security provides authentication manager and we can configure it as below. Even tough we have hardcoded the user name and passwords we can further configure this with database, identity store, ldap or active directory.

    ```xml
        <authentication-manager>
            <authentication-provider>
                <user-service>
                    <user name="user1" password="user1Pass" authorities="ROLE_USER"/>
                    <user name="admin1" password="admin1Pass" authorities="ROLE_ADMIN"/>
                </user-service>
            </authentication-provider>
        </authentication-manager>
    ```

    Then we can have helper method to authenticate user credentials as follows

    ```java
    public static Authentication auth(AuthenticationManager authenticationManager, String userName, String password) {
        SecurityContext sc = SecurityContextHolder.getContext();
        UsernamePasswordAuthenticationToken upToken = new UsernamePasswordAuthenticationToken(userName, password);
        Authentication authentication = authenticationManager.authenticate(upToken);
        sc.setAuthentication(authentication);
        return sc.getAuthentication();
    }
    ```

3. When Spring container is started and load-on-startup is 1, then it starts thread pool, web server, servlet context, servlet mappings etc        along with a map for storing HTTPSession objects <br> `Outer<String, HttpSession<String, Object>`. This map holds all active clients          session data with key as `JSESSIONID` value of cookies.
   Now to access HttpSession we have to

   1.  Getting session object      
       ```java
        HttpSession session = request.getSession(true);
       ``` 
       If flag is true Web Container(WC), will check if there is existing Httpsession object
       ```java
        Cookie[] cookies = request.getCookies();
        for(Cookie cookie: cookies) {
            if(cookie.getName().equals("JSESSIONID")) {
                return Outer.get(c.getValue());
            }
        }
        return null;
       ``` 
       If return value is null WC will check in URL for jsession id value. If not the new HttpSession and cookie with jsession id is created. This cookie is added to response and Outer map.

   2. Adding attributes to session - We can add objects to session to be remembered throughout session.
      ```java
       session.setAttribute("user_details", user)
      ```  
      `@Scope(value="session")` annotation can be applied to bean to have life cycle of bean throughout the session.

   3. Redirecting client - As cookies are already there in response, in further requests we track user activites through session
      ```java
       response.sendRedirect("loginSuccessfullPage"); 
      ```  

   4. Invalidating session 
      ```java
       HttpSession session = request.getSession(false);
       if (session !== null) {
           session.invalidate();
       } 
      ```  
      The false flag will give existing session if exists and will not create new session object. WC removes the entry from Outer map.  

This is how sessions can be managed in traditional web apps. The server itself deals with HttpSession objects(Outer map) and this data is stored in server memory. In HttpSession we can save any object even huge dataset or datatables. If there are more number of active clients, usage of huge memory at server side can cause performance issues. Hence nowadays in modern web apps session are not maintained. As server applications are becoming more service oriented and are exposing RESTful services, each call to REST API is authenticated by tokens and no sessions are needed to be created to make it stateless. 

##### <b>REST APIs - Stateless</b>
REST(`Re`presentaional `S`tate `T`ransfer) - In REST apis, server does not store any session or client state, and the request from client should have all the necessary information. This solved the problem of server scalablity and if we need to store or persist any data, it should be done on client side. In browsers we generally use local storage and in mobile applications we use SQlite database to persist client specific data. Also this approach helped to separate user inteface and data storage concerns.