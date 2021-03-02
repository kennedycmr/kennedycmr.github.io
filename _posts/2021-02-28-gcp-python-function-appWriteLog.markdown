---
layout: post
title:  "GCP - Python Function to Write Logs to Cloud Logging"
date:   2021-02-28 18:56:00 -0500
categories: gcp python function
tags: gcp python function appWriteLog
---
Below is an example python function to write logs back to the Cloud Logging service in a standardized way. I use this when I need to ensure consistency in how logs are written so they can be easily found. 

Note: the OS environment variables are optional but we use them for consistent logging.

{% highlight python %}
### Logging Setup 
import os, logging, google.cloud.logging
from google.cloud.logging_v2.resource import Resource

### Load OS Environment variables
functionName = os.environ.get('functionName', '"functionName" environment variable not set')
gcpRegion    = os.environ.get('gcpRegion', '"gcpRegion" Specified environment variable not set')
gcpProjectId = os.environ.get('gcpProjectId', '"gcpProjectId" environment variable not set')

### Setup Logging
log_client = google.cloud.logging.Client(project=gcpProjectId)
log_name   = 'cloudfunctions.googleapis.com%2Fcloud-functions' 
logger     = log_client.logger(log_name.format(gcpProjectId))


def appWriteLog(transactionId, severity, text):
    logLabels = {"message": text, "function_name": functionName, "region": gcpRegion, "transactionId": transactionId}    
    res = Resource(type="cloud_function", labels=logLabels)
    try:
        logger.log_struct(
            {"transactionId":transactionId, "message": str(text)}, 
            resource=res, 
            severity=severity
        )
        return True
    except:
        return False

def appWriteInfo(transactionId, text) :  # backward compatibility
    result = appWriteLog(transactionId, 'INFO', str(text))
    return result

def appWriteWarning(transactionId, text) : # backward compatibility
    result = appWriteLog(transactionId, 'WARNING', str(text))
    return result

def appWriteError(transactionId, text) : # backward compatibility    
    result = appWriteLog(transactionId, 'ERROR', str(text))
    return result
{% endhighlight %}
