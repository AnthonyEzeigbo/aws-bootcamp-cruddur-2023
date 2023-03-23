# Week 2 — Distributed Tracing
## Required Task/Home work

This task is done using `Gitpod` workspace

1. **HOW TO SETUP HONEYCOMB**

Before setting up Honeycomb i will first of all explain what honeycomb is and its usage, honeycomb is an observability tool, which gives its users the ability to 
Measure,monitor and understand the internal state of and behaviour of a system based on its external output.
usage: it is used to investigate problems and there patterns in your appliction and also help in honeing those problems.


Now lets setup our honeycomb environment

step 1: log into your honeycomb account and if you dont have one yet you can create an account using this [link](https://www.honeycomb.io/)
In the logon menu click on the section that says `ENVIRONMENT` on the left hand corner of your page.

![](assets%20week2/Honeycomb/honey%20comb.PNG)


Click `Manage Environments` to create one. Name could be "anything", I will name mine `bootcamp` as you can see that at the top of this page i have already created my 
`environment` already, once you do that you should be able to see you environment created

As you can see i have `API KEYS` that attached to the name of my environments one is for `bootcamp` and the other is for `test` now click on `View API Keys` that is for bootcamp and copy it.


The API keys you use determines the environment that data will land in, so be carefull not use a wrong api key always try to verify it before it is used
Export that copied `API KEY` to `GITPOD` using the command below.


```
# to export it to our gitpod terminal 
export HONEYCOMB_API_KEY="..."

# confirm export 
env | grep HONEY

# to have it permanently saved unto gitpod env variables
# when it starts next time, we don't have to export it again  
gp env HONEYCOMB_API_KEY="..."

```


2. **Instrument backend flask to use OpenTelemetry (OTEL) with Honeycomb.io as the provider**

we setup honeycomb for our `backend` by hardcoding it, and make sure you are not consistant across multiple services, honeycomb uses the  `OTEL` libraries which  simply means `OpenTelemetry` libraries. 

Let's set our honeycomb env variable for our `backend` in the `docker-compose.yml` file. Add the following lines of code to the file, we use the `OTEL_SERVICE_NAME` inside our `docker-compose-file` by doin this you are configuring `OTEL` TO send messsage to honeycomb
it is done using the cmd below
```
# honeycomb env variables 
OTEL_SERVICE_NAME: 'backend-flask'
OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
```

You also have to install the `OTEL` libraries into your appliction, by adding the following lines to the `requirements.txt` file located in the `backend-flask/` directory


```
opentelemetry-api 
opentelemetry-sdk 
opentelemetry-exporter-otlp-proto-http 
opentelemetry-instrumentation-flask 
opentelemetry-instrumentation-requests # this should instrument outgoing HTTP calls

```


 you can now install the dependencies you listed in the `requirements.txt` file In the `backend-flask` directory, run the following command:


```
pip install -r requirements.txt
```



using python script we installed `opentelemetry API` in the backend of our APP which in our case its  cruddur 
Now you can create and initialize honeycomb by adding the following lines of code to app.py file

we created another python script for the tracer 

```
# Honeycomb
from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor

# Initialize tracing and an exporter that can send data to Honeycomb
provider = TracerProvider()
processor = BatchSpanProcessor(OTLPSpanExporter())
provider.add_span_processor(processor)
trace.set_tracer_provider(provider)
tracer = trace.get_tracer(__name__)

# Initialize automatic instrumentation with Flask
app = Flask(__name__) # if this link already exists, DON'T call it again
FlaskInstrumentor().instrument_app(app)
RequestsInstrumentor().instrument()
```
After the updates, test out your configuration by running this command:  `docker compose up`


[verify Dataset setup on your honeycomb user page](https://ui.honeycomb.io/anthonyp24u-gettingstarted/environments/bootcamp/datasets)


Confirm that honeycomb is getting your data. If not, here are some steps for troubleshooting

  **#** First, check what API key your environment is using. Do that by checking your env variables  `env | grep HONEYCOMB_API_KEY`
  
  **#** Second, copy that API key to this link [Troubleshoot Honeycomb](https://honeycomb-whoami.glitch.me/) to debug what environment is associated with that key.
  
  **#** Third, navigate to the appropriate environment, and you should see your data. You can go ahead and explore the traces and its spans.
  
  
  3. **Working with traces/spans/attribute in Honeycomb.io**

Tracers are sent to honeycomb using the name of the tracer like  `home_activities` we will set it up as our `library.name`
it can be used to show all spans created from a particular tracer.

**How To Configure a Tracer**

Add the following lines of code to your `backend-flask/services/home_activities.py` file:


```
from opentelemetry import trace

# add before the HomeActivities class
tracer = trace.get_tracer("home.activities") # the tracer name here is home_activities
```

**Creating a Span*

After craeting a tracer we then create a span to discribe what incident took place in inside our application.

Create a span with our configured tracer. A span simply describes what is happening in your application.

Add the following lines of code to your `backend-flask/services/home_activities.py` file:

```
# add under def run():
with tracer.start_as_current_span("home-activities-mock-data"):
# make sure every other line beneath is properly indented under the code you pasted 
```

After the updates, test out your configuration by running this command:  `docker compose up`

To create some spans, append this URL to your backend, ...`/api/activities/home`

![](assets%20week2/Honeycomb/honeycomb%20span.PNG)


**Added   span Attributes**

These attributes gives us more context to our logs. Go ahead and add a few by including these lines of code to your` backend-flask/services/home_activities.py` file:

```
# in the def run(): section
# add attribute -> app.now 
span = trace.get_current_span()
span.set_attribute("app.now", now.isoformat())

# at the bottom -> app.result_length
span.set_attribute("app.result_length", len(results))
```


![](assets%20week2/Honeycomb/honeycomb%20creating%20span%20and%20tracing.PNG)



After the updates, test out your configuration by running this command:  `docker compose up`


4. **Run queries to explore traces within Honeycomb.io**

With our previously hard-coded attributes `app.now` and `app.result_length`, let's create and run some queries

![Query](assets%20week2/Honeycomb/honeycomb%20trace.PNG)








![](assets%20week2/Honeycomb/Honeycomb%20maps.PNG)









5.**Instrument AWS X-ray into the backend**

AWS X-ray makes it easy for dev's to analyze the behavior of the distributed  applications by providing request, tracing, exception collection and profiling capabilities, it provides a complete view of requests as they travel through your application and filters visual data across payloads, with low-code or no-code motions.


  **How to install X-ray**

for x-ray the first thing we have to do is to install the [AWS SDK](https://aws.amazon.com/sdk-for-javascript/)
go to the cruddur folder open up the backend-flask file, open up the `requirment.txt` file and add the `aws-xray-sdk` to the file
Add the following lines to the `requirements.txt` file located in the `backend-flask/` directory




![](assets%20week2/aws-xray/x-ray/aws-sdk%20-%20Copy.PNG)

```
aws-xray-sdk
```

Go to the terminal and install the dependencies you listed in the `requirements.txt file In the `backend-flask` directory, run the following command:
```
pip install -r requirements.txt
```


**Instrument X-ray for flask**
Attached the AWS-sdk source code  to your `app.py`
To instrument our backend, add the following lines of code to the `backend-flask/app.py`

```
from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.ext.flask.middleware import XRayMiddleware

xray_url = os.getenv("AWS_XRAY_URL")
xray_recorder.configure(service='backend-flask', dynamic_naming=xray_url)

XRayMiddleware(app, xray_recorder)
```


![](assets%20week2/aws-xray/x-ray/x-ray%20for%20flask.PNG)


**create sample**

Create a `json` file in the `aws/json` directory

```
touch aws/json/xray.json
```

Add the following lines of code to your newly created file:


```
{
  "SamplingRule": {
      "RuleName": "Cruddur",
      "ResourceARN": "*",
      "Priority": 9000,
      "FixedRate": 0.1,
      "ReservoirSize": 5,
      "ServiceName": "backend-flask",
      "ServiceType": "*",
      "Host": "*",
      "HTTPMethod": "*",
      "URLPath": "*",
      "Version": 1
  }
}
```

Let's create an x-ray trace group and a sampling rule. Go ahead and run the following command:

```
# create a trace group in AWS x-ray
aws xray create-group \
   --group-name "Cruddur" \
   --filter-expression "service(\"backend-flask\")"

# create a sampling rule
aws xray create-sampling-rule --cli-input-json file://aws/json/xray.json
```

Configure X-ray daemon with docker-compose

Setup the daemon in the `docker-compose.yml` file by adding these following lines:

```
# add these env variables above in the ENV section
AWS_XRAY_URL: "*4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}*"
AWS_XRAY_DAEMON_ADDRESS: "xray-daemon:2000"

xray-daemon:
    image: "amazon/aws-xray-daemon"
    environment:
      AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
      AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
      AWS_REGION: "us-east-1"
    command:
      - "xray -o -b xray-daemon:2000"
    ports:
      - 2000:2000/udp
 ```
      
      
  After the updates, test out your configuration by running this command:  `docker compose up`
  
  Check your x-ray container logs to make sure logs were successfully sent to AWS X-ray.
  
  
  ![x-ray trace](assets%20week2/aws-xray/x-ray/x-ray%20traces.PNG)
  
  
  
  
  
6. **Create a custom segment and subsegment with AWS X-ray**

In the `backend-flask/services/user_activities.py` file, add the following contents:

```
from aws_xray_sdk.core import xray_recorder

# Add in the def run(user_handle): section, 
# but below, before the return statement
# Start a segment
	subsegment = xray_recorder.begin_segment('mock-data')

    dict = {
      "now": now.isoformat(),
      "results-size": len(model['data'])
    }

    subsegment.put_metadata('key', dict, 'namespace')

    # Close subsegment
    xray_recorder.end_subsegment()
 ```
 
 
 7. **Install WatchTower & write custom logger to send app log data to CloudWatch Log Group**


Add the following lines to the `requirements.txt` file located in the `backend-flask/` directory

```
watchtower
```

Now install the dependencies you listed in the `requirements.txt` file In the `backend-flask` directory, run the following command:


```
pip install -r requirements.txt
```

**Set environment variables for watchtower**

In the `docker-compose.yml` file, add the following lines:

```
AWS_DEFAULT_REGION: "${AWS_DEFAULT_REGION}"
AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
```

Now let's configure our CloudWatch logger. Add the following lines to the `app.py` file:


we first of all setup a log_group called `cruddur` in the `app.py` file, after every single request if we want to login an error
we have to use the code below, to do error logging.
```
# CloudWatch Logs
import watchtower
import logging
from time import strftime

# Configuring Logger to Use CloudWatch
LOGGER = logging.getLogger(__name__)
LOGGER.setLevel(logging.DEBUG)
console_handler = logging.StreamHandler()
cw_handler = watchtower.CloudWatchLogHandler(log_group='cruddur')
LOGGER.addHandler(console_handler)
LOGGER.addHandler(cw_handler)
LOGGER.info("test message")
```


At the section where you see `@app.route`  you have to paste the code below in there it:  

```
@app.after_request
def after_request(response):
    timestamp = strftime('[%Y-%b-%d %H:%M]')
    LOGGER.error('%s %s %s %s %s %s', timestamp, request.remote_addr, request.method, request.scheme, request.full_path, response.status)
    return response
```
      


Now let's log something in one of the API endpoint by adding the following lines to our `backend-flask/services/home_activities.py` file:


```
# in the class HomeActivities: section, update and add these lines
def run(logger):
    logger.info("HomeActivities")
```

In the `app.py` file, update this section of your code to look like this:


```
@app.route("/api/activities/home", methods=['GET'])
def data_home():
  data = HomeActivities.run(logger=LOGGER)
  return data, 200
```




Then run `docker compose up` to verify and test your confuguration




![cloudwatch log](assets%20week2/aws-xray/x-ray/cloudwatch%20log%20group.PNG)


8. **Integrate Rollbar for Error Logging**

Rollbar is a cloud-based bug tracking and monitoring solution that caters to organizations of all sizes.

Add the following lines to the `requirements.txt` file located in the `backend-flask/` directory

```
blinker 
rollbar
```

install the dependencies you listed in the `requirements.txt` file In the `backend-flask` directory, run the following command:

``
pip install -r requirements.txt
```


add the rollbar access token

```
export ROLLBAR_ACCESS_TOKEN=""
gp env ROLLBAR_ACCESS_TOKEN=""
```


