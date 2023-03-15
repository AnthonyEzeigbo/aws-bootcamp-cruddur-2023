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
