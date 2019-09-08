---
layout: page
title: Authentication Adventures
permalink: /authentication-adventures/
---

This part took awhile to wrap my head around.


For registering a new user:

1. POST to /api/V1/user/token
2. POST to api/V1/user/register
2. 

For logging in a user:

1. POST to /api/V1/user/token 
{% highlight javascript %}
getToken() {
    let options = {
        headers: new HttpHeaders({ "Content-Type": "application/json" })
    };
return this.http
    .post("http://personifier.dd:8083/api/V1/user/token.json", "", options)
    .subscribe(resp => {
    this.userTokenString = resp["token"];
    });
}
{% endhighlight %}
    - (gets you a token to user only for logging in)
2. POST to /api/V1/user/login 
    - (include the token you just got in header information as "X-CSRF-Token":token)
    - (include username and password object as body)
3. You get a new token after logging in. Use this token from now on. It's your session token.
4. Save the new session token in localStorage
5. Save the sessid in localStorage
6. Save the session_name in localStorage
7. Now you can POST to /system/connect/ and include 
    - new session token in headers as "X-CSRF-Token":session_token
    - New header "X-Cookie":session_name=sessid 
    - gets the logged in user and retruns their user object back.
    - Can set this.currentUser in AuthService in the constructor function by calling POST to /system/connect/
    - This way if the user reloads the browser, the user is still logged in client side and server side

8. In your auth service, in the constructor you can run a function checkAuthorizationStatus(); which will user