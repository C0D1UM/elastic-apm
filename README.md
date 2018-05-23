# Elastic APM (Application Performance Matric)
Template for setting up Elastic APM with docker-compose

## Setting Up Server
Elastic Search required virtual memory up to 263MB, so we need to
```bash
$ sudo sysctl -w vm.max_map_count=262144
```

That all we need to and now we can up it with docker-compose (make sure you install docker and docker-compose in your machine).
```bash
$ docker-compose up -d
```

## Setting APM Agent
Agent is simply your application that you would like to connect to the APM service.

In this, we will show example with Django. For other framework you can find it here [https://www.elastic.co/solutions/apm](https://www.elastic.co/solutions/apm).

Install elastic-apm with pip
```bash
$ pip install elastic-apm
```

Add elasticapm app to your app setting
```python

INSTALLED_APPS = [
  ...
  'elasticapm.contrib.django'
  ...
]
```

Adding middleware classes
```python
MIDDLEWARE = (
  # elasticapm middleware must be at the top of middleware list
  'elasticapm.contrib.django.middleware.TracingMiddleware',
  ...
)
# if you are using Django version lower than 1.11 your probably add elasticapm
# middleware to this
MIDDLEWARE_CLASSES = (
  'elasticapm.contrib.django.middleware.TracingMiddleware',
  ...
)
```

APM connection configuration
```python
ELASTIC_APM = {
    # allowed characters in SERVICE_NAME: a-z, A-Z, 0-9, -, _, and space
    'SERVICE_NAME': 'my-service-name-production',
    
    # SECRET_TOKEN can be anything, doesn't required to generate from apm.
    'SECRET_TOKEN': 's3cr3t-t0k3n',
    
    # SERVER_URL is the url that your apm server is running
    'SERVER_URL': 'http://apm.example.com:8200',
    
    # DEBUG default is False, but if you want apm to send error event
    # it's in debug mode you can set to True.
    'DEBUG': True,
}
```

## Check the Configuration
You can check if all the settings configuration for apm is correct with
```bash
$ python manage.py elasticapm check
```

You can also test if the error report is sent to apm server with
```bash
$ python manage.py elasticapm test
```
