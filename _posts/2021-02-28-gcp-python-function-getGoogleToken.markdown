---
layout: post
title:  "GCP - Python Function to Obtain Google Token"
date:   2021-02-28 18:56:00 -0500
categories: gcp python function
tags: gcp python function
---
Below is an example python function to obtain a Google token. I use this when I need to perform REST API calls to other GCP services. The token is my authorization to use those services. 

Note: The transactionId is only used for logging and tracking. It can be removed along with the call to appWriteError which is a standard logging function.

{% highlight python %}
import google.auth
import google.auth.transport.requests

def getGoogleToken(transactionId):
    # Auth scopes URL from: https://cloud.google.com/cloud-build/docs/api/reference/rest/v1/projects.triggers/run
    google_auth_scopes = 'https://www.googleapis.com/auth/cloud-platform'
    try: 
        credentials, project = google.auth.default(scopes=[google_auth_scopes])
        request = google.auth.transport.requests.Request()
        credentials.refresh(request)
        return credentials.token
    except :
        appWriteError(transactionId, 'Failed to get access token')
        return 1
{% endhighlight %}
