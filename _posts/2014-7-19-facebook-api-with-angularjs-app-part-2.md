---
layout: post
title:  Using Facebook API with AngularJs app - Part 2
---

[<img class="size-medium wp-image-845 aligncenter" title="AngularJS Facebook" alt="Facebook API with AngularJs app" src="http://www.boynux.com/wp-content/uploads/2014/03/angularjs-facebook-300x120.png" width="300" height="120" />][1]

Hi, This is the second part of "Facebook API with AngularJs app". If you haven't read the [first part][2] yet, I suggest you to read that first. 

## 1. Initial setup

You can find explanations in the <a href="http://www.boynux.com/facebook-api-with-angularjs-app-part-1/" title="Using Facebook API with AngularJs app – Part 1" target="_blank">first part</a>, here I just show the code. 

```javascript
 angular.module ('myFacebookApp', ['bnx.module.facebook']);
```

And we need this HTML snippet somewhere in our document body. This directive is actually loads Facebook Javascripts. 

```javascript
<div ng-app="myFacebookApp">
    <facebook app-id="<app-id>"></facebook>
</div>
```

We are done! Very simple :) Now we have initialized Facebook API and it's ready to be used.

<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.15/angular.min.js"></script> 
<script src="http://www.boynux.com/wp-content/uploads/2014/05/facebook.js"></script>

<script language="javascript">
var app = angular.module ('myFacebookApp', ['bnx.module.facebook']);
app.controller('mainCtrl', function ($scope, facebook) {
    $scope.$on('fb.auth.authResponseChange', function (event, response) {
        $scope.$apply (function () {
            $scope.connected = (response.status == 'connected');

            if ($scope.connected) {
                facebook.api ('me').then (function (result) {
                    $scope.user = result;
                });
            } else {
                $scope.user = null;
            }
            });
        });
    });
</script> 

## 2. Login button

<script async src="//pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script> 
<!-- Display Large Rectangle -->
<ins class="adsbygoogle" 
    style="display:inline-block;width:336px;height:280px" 
    data-ad-client="ca-pub-7360583392867579" 
    data-ad-slot="7819924448">
</ins> 
<script> (adsbygoogle = window.adsbygoogle || []).push({}); </script>

I want a Facebook login button in my app, so users can click and login using their Facebook account. [Facebook doesn't reveal account credentials to 3rd party apps]. We can get from a very basic information of user's public profile to full access to all photos and posts and albums. For purpose of this app I only setup minimum requirement which is "Basic Info access". If you want to access more advanced information you've to submit your app to be reviewed by Facebook first. 

Here is the Angular Directive for Facebook login button: 

```javascript
    <facebook-login auto_logout="true" scope="basic_info"> </facebook-login>
```

<facebook-login auto_logout="true" scope="basic_info"></facebook-login> 

That's it. This snippet will show a standard medium size Facebook login button and grants `basic_info` access when user logs into her Facebook account. If you need more details on this please refer to mentioned Github repository. All details explained in code documents. Now you can see a login button right here. 

<div ng-app="myFacebookApp">
  <facebook app-id="1491187207767298"></facebook> 
    <facebook-login scope="basic_info" auto_logout="true">
  </facebook-login> 
  <br /> <br /> 
  <div ng-controller='mainCtrl'>
    <div ng-if='!connected'>
      <p>
        <strong>Wait, please login using above button to continue reading...</strong>
      </p>
    </div>
    
    <div ng-if='connected'>
      <p>
        <img src="http://graph.facebook.com/{{user.id}}/picture?type=square" /> Thanks, <strong>{{user.name}}</strong>
      </p>
      
      <h2>
        3. Control Login action
      </h2>
      
      <p>
        Now we successfully added Login button to our page. You are reading these lines, thus I manged to capture your login status and display some hidden parts of this article. Here I explain how you can achieve that using this API.
      </p>
      
      <p>
        When user clicks on Login button, Facebook API opens a small window in order to ask user if she wants to login to this web site using her Facebook account. If user accepts this request, Facebook authentication state for that user will be changed to '<code>connected</code>'. AngularFacebook API will broadcast '<code>fb.auth.authResponseChange</code>' to inform app about this change. knowing all that, by listening to this event we can update our app according to user's new status.
      </p>
      
      <p>
        Here is the javascript code which is used in this article to show/hide this section of article according to user's login state.:
      </p><pre class='prettyprint' lang-js'> app.controller('mainCtrl', function ($scope, facebook) { $scope.$on('fb.auth.authResponseChange', function (event, response) { $scope.$apply (function () { $scope.connected = (response.status == 'connected'); }); }); }); </pre> 
      
      <p>
        Inside angular controller I used '<code>$scope.$on</code>' to bind a listener on '<code>fb.auth.authResponseChange</code>' event. The event handler receives an event object and Facebook api response as first and seconds arguments respectively. By examining '<code>response.status</code>' we know whether user is currently logged in our app not. Why I put assignment inside '<code>$apply</code>' is because I need to inform angular about <code>$scope</code> changes.
      </p>
      
      <h2>
        4. Get user basic info
      </h2>
      
      <p>
        If you've noticed after logging into Facebook app, I showed your name right below Login button. With AngularFacebook API this is also very simple. I used <code>api</code> function, to fetch that information. This code goes into above controller code, inside listener function right after <code>$scope.connected = (response.status == 'connected');</code> line.
      </p><pre class='prettyprint' lang-js'> if ($scope.connected) { facebook.api('me').then (function (result) { $scope.user = result; }); } else { $scope.user = null; } </pre> 
      
      <p>
        This code checks if user is already logged into app (by examining <code>connected</code> property of <code>$scope</code> from previous line), then fetches user information by calling <code>api</code> function. passing '<code>me</code>' as parameter which means we are interested, in currently logged in user information. <code>user.name</code> contains user's name. For more details on user information object please refer to <a href="https://developers.facebook.com/docs/graph-api/reference/v2.0/user" title="Facebook /user dcoument" target="_blank">Facebook documentations</a>.
      </p>
      
      <h2>
        5. Conclusion
      </h2>
      
      <p>
        Using AngularFacebook library is an easy way to utilize both Facebook API and AngularJs framework and the same time. Integrating these two libraries is no longer a problem and time consuming task. <br /> If you have any enquiries about this library or want to add/request new features please <a href="http://www.boynux.com/im-here/" title="You can find me here …" target="_blank">contact</a> me or submit a merge request on it's <a href="https://github.com/boynux/AngularFacebook" title="AngularFacebook on Github" target="_blank">Github</a> repository.
      </p>
      
      <p>
        <strong>In next article (3rd part) I'll show how you can post information on user's wall. </strong>If you want to get informed you can subscribe to my mailing list using 'Subscription' form in footer of this page. You'll only receive email about my new articles only.
      </p>
      
      <p>
        Thank you for reading this article, if you've found these articles useful please share using buttons below. You are very welcome to leave comments using provided form at button of this page.
      </p>
    </div>
  </div>
</div>

<!-- Responsive Display -->
<ins class="adsbygoogle" 
    style="display:block" 
    data-ad-client="ca-pub-7360583392867579" 
    data-ad-slot="4587256441" 
    data-ad-format="auto">
</ins> 
<script> (adsbygoogle = window.adsbygoogle || []).push({}); </script>

[1]: http://www.boynux.com/wp-content/uploads/2014/03/angularjs-facebook.png
[2]: http://www.boynux.com/facebook-api-with-angularjs-app-part-1/ "Using Facebook API with AngularJs app – Part 1"
