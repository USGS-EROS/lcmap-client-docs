# API: Models

The LCMAP Science Execution Environment provides a highly parallel and scalable system to apply science models to batches and streams of data. The associated services have been designed for the ability to be distributed globally. The execution environment handles the efficient and fair scheduling of execution  science models in a widely distributed data-intensive environment.



<aside class="info">
All of the models return a hypermedia link immediately, adhering to the principles of [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS).
</aside>

The supported models to-date are the following:

* Sample models:
 * OS process model <span class="status-alpha">[ALPHA]</span>
 * Piped processes model <span class="status-alpha">[ALPHA]</span>
 * Sandboxed model <span class="status-in-progress">[ALPHA]</span>
 * Distributed sanbox model <span class="status-not-started">[NOT STARTED]</span>
 * Distributed process model <span class="status-not-started">[NOT STARTED]</span>
* Continuous Change Detection and Classification (CCDC) <span class="status-initial-stages">[INITIAL STAGES]</span>

<aside class="caution">
In order to execute models on the LCMAP system, you have to have been granted permission to do so for the specific model in question.
</aside>


## SAMPLE MODELS

The "sample" models provided by LCMAP are for testing and demonstration purposes, not for performing meaningful computations. They offer LCMAP API developers a means for exploring forth-coming features in client libraries; giving new users a sense of what is possible; and inspiring scientists to contemplate how their computational models might be included in LCMAP.


## &bull; Sample: OS Process Model

> Execute the sample process model for the year ``2017``, setting the results to delay for 2 minutes:

```shell
$ RESULT_PATH=$(curl -s -X POST \
    -H "$LCMAP_VERSION_HDR" -H "$LCMAP_TOKEN_HDR" \
    -d "delay=120" -d "year=2017" \
    "${LCMAP_ENDPOINT}/api/models/sample/os-process" | \
    jq -r '.body.result.link.href')
$ echo $RESULT_PATH
/api/jobs/sample/os-process/6d65033bb2007959174dd284ea8070f4
```

```python
>>> from lcmap.client import Client
>>> client = Client()
>>> response = client.models.samples.os_process.run(year=2017, delay=120)
>>> response.result["link"]["href"]
u'/api/jobs/sample/os-process/6d65033bb2007959174dd284ea8070f4'
```

```vb
TBD
```

```clojure
=> (require '[lcmap.client.models.sample-os-process :as sample-model])
nil
=> (def result (sample-model/run client :year 2017 :delay 120))
#'result
=> (get-in result [:result :link :href])
"/api/jobs/sample/os-process/6d65033bb2007959174dd284ea8070f4"
```

```ruby
TBD
```

> Check on the status of the model run:

```shell
$ curl -v -H "$LCMAP_VERSION_HDR" -H "$LCMAP_TOKEN_HDR" \
    "${LCMAP_ENDPOINT}${RESULT_PATH}" \
    jq .body.result
...
< HTTP/1.1 202 Accepted
...
"pending"
```

```python
# When you know the result contains a link, you can use the follow_link method
>>> response.follow_link().result
'pending'
# If you would like to see all of the response data, you can do this:
>>> response.follow_link().data
{'errors': [], 'result': 'pending', 'status': 200}
```

```vb
TBD
```

```clojure
=> (require '[lcmap.client.http :as http])
; When you know the result contains a link, you can use the follow-link
; function
=> (http/follow-link client result)
{:result "pending"}
```

```ruby
TBD
```

```r
TBD
```

> After the job has finished, ``GET``ing the result resource will return actual data:

```shell
$ curl -v -H "$LCMAP_VERSION_HDR" -H "$LCMAP_TOKEN_HDR" \
    "${LCMAP_ENDPOINT}${RESULT_PATH}"
...
< HTTP/1.1 200 OK
...
{"result":"                             2017\n\n
  ...}
```

```python
>>> response.follow_link().result
'                            2017\n ...'
```

```vb
TBD
```

```clojure
=> (http/follow-link client result)
{:result_id "6974c65f54d3cfb453d8137714bcc741"
 :result "                            2017\n ..."}
```

```ruby
TBD
```

```r
TBD
```

This sample model simply executes an arbitrary binary (in this case, the ``cal`` program) as an OS process on a single core.

<aside class="info">
Note that subsequent calls with the same parameters will return immediately, since the results are automatically stored in the database and checked before executing a model.
</aside>

## &bull; Sample: Piped Processes Model

> Execute the sample piped processes model with the default values for the supported flags:

```shell
$ RESULT_PATH=$(curl -s -X POST \
    -H "$LCMAP_VERSION_HDR" -H "$LCMAP_TOKEN_HDR" \
    "${LCMAP_ENDPOINT}/api/models/sample/piped-processes" | \
    jq -r '.body.result.link.href')
$ echo $RESULT_PATH
/api/jobs/sample/piped-processes/5d45974eabb4ff1060f6278555d99375
```

```python
>>> from lcmap.client import Client
>>> client = Client()
>>> response = client.models.samples.piped_processes.run()
>>> response.result["link"]["href"]
u'/api/jobs/sample/piped-processes/feeb956a9a08de4b60b92fef6f832d43'
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

```r
TBD
```

> You can also pass parameters:

```shell
$ RESULT_PATH=$(curl -s -X POST \
    -H "$LCMAP_VERSION_HDR" -H "$LCMAP_TOKEN_HDR" \
    -d "number=true" -d "count=true" -d "words=true"\
    "${LCMAP_ENDPOINT}/api/models/sample/piped-processes" | \
    jq -r '.body.result.link.href')
$ echo $RESULT_PATH
/api/jobs/sample/piped-processes/5d45974eabb4ff1060f6278555d99375
```

```python
>>> from lcmap.client import Client
>>> client = Client()
>>> response = client.models.samples.piped_processes.run(
        number=True, count=True, words=True)
>>> response.result["link"]["href"]
u'/api/jobs/sample/piped-processes/2441fca31a583a0c2bb4aa091607775d'
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

```r
TBD
```

> After the job has finished you can extract the data from the result:

```shell
$ curl -v -H "$LCMAP_VERSION_HDR" -H "$LCMAP_TOKEN_HDR" \
    "${LCMAP_ENDPOINT}${RESULT_PATH}" \
    jq .body.result
...
< HTTP/1.1 200 OK
...
"47\n"
```

```python
>>> response.follow_link().result
u'47\n'
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

```r
TBD
```

The sample "piped process" model demonstrates the execution of a chain of tasks (command line utilities), each of which may take optional parameters. This is a common workflow for science scripters and thus important to demonstrate.

In this particular case, we simple ``cat`` the output of ``/etc/hosts``, pipe that to ``uniq``, and then pipe *that* output to ``wc``. Each of the command line tools in this chain may take optional boolean parameters:

* Passing data ``number=true`` will cause the ``--number`` flag to get set for ``cat``, prepending incrementing integers to each line of output.
* Passing data``count=true`` will cause the ``--count`` flag to get set for ``uniq``, prefixing each line with the number of occurences.
* Pasing data ``bytes=true``, ``words=true``, or ``lines=true`` will cause one of the ``--bytes``, ``--words``, or ``--lines`` flags (respectively) to get set for ``wc``.

<aside class="info">
Note that subsequent calls with the same parameters will return immediately, since the results are automatically stored in the database and checked before executing a model.
</aside>

## &bull; Sample: Sandboxed Model

> Execute the sample sandboxed model for the year ``2017``:

```shell
$ RESULT_PATH=$(curl -s -X POST \
    -H "$LCMAP_VERSION_HDR" -H "$LCMAP_TOKEN_HDR" \
    -d "docker-tag=usgs-lcmap/debian-docker-sample-process" \
    -d "year=2017" \
    "${LCMAP_ENDPOINT}/api/models/sample/docker-process" | \
    jq -r '.body.result.link.href')
$ echo $RESULT_PATH
/api/jobs/sample/os-process/439ae2866a39bb5cbbe934583bfef114
```

```python
>>> from lcmap.client import Client
>>> client = Client()
>>> response = client.models.samples.docker_process.run(
        year=2017, docker_tag="usgs-lcmap/debian-docker-sample-process")
>>> response.result["link"]["href"]
u'/api/jobs/sample/docker-process/1d674fd3b6d7973fbb2cdc9d1cd7f012'
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

```r
TBD
```

> After the job has finished you can extract the data from the result:

```shell
$ curl -v -H "$LCMAP_VERSION_HDR" -H "$LCMAP_TOKEN_HDR" \
    "${LCMAP_ENDPOINT}${RESULT_PATH}" \
    jq .body.result
...
< HTTP/1.1 200 OK
...
"                             2017\n ..."
```

```python
>>> response.follow_link().result
'                            2017\n ...'
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

```r
TBD
```

Similar in nature to the OS Process Sample Model, this sample model executes a
"sandboxed" science model by running a single Docker container. This sample is useful in that it
provides an example of the simplest case where a parameterized Docker image may
be executed remotely via API calls and then both storing and returning the
computed results.

However, in order to do this a certain amout of setup needs to be done. In
particular:

* ``git clone`` the [LCMAP Dockerfiles](https://github.com/USGS-EROS/lcmap-dockerfiles) repository
* Build the sample Docker image with ``make sample-docker-model`` (from inside the ``lcmap-dockerfiles`` directory)
* Make sure that the Docker image is built on the machine where the LCMAP REST server is running

Note that the sample model for running a Docker process on the LCMAP REST
server is essentially executing the command ``docker run -t usgs-lcmap/debian-docker-sample-process --year 2017``,
or whatever docker tag and year you happen to pass as arguments. To be clear,
this is an example of running a parameterized Docker image taking the single
parameter ``year`` which gets passed to the Docker entrypoint on the running
container.

Full science models that use parameterized Docker containers will support a
(potentially) great number of parameters to be passed to a running container in
order to execute the science model appropriately (as intended by its creators).

<aside class="info">
Note that subsequent calls with the same parameters will return immediately, since the results are automatically stored in the database and checked before executing a model.
</aside>


## &bull; Sample: Distributed Sandbox Model

> Execute the sample Docker Mesos model for the year ``2017``:

```shell
TBD
```

This sample model executes a Docker container across agent nodes in a Mesos cluster. Model parameters are split across a configurable number of nodes and results are combined in the final step of model execution.

<aside class="info">
Note that subsequent calls with the same parameters will return immediately, since the results are automatically stored in the database and checked before executing a model.
</aside>


## &bull; Sample: Distributed Process Model

> Execute the sample Mesos framework model for the year ``2017``:

```shell
TBD
```

```python
TBD
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

```r
TBD
```


This sample model executes a Mesos framework which has been tuned to parallelize on both the science model parameters and the required queries to the data warehouse. It is this combination of factors which are used to split work across a configurable number of nodes. Results are combined in the final step of model execution.

<aside class="info">
Note that subsequent calls with the same parameters will return immediately, since the results are automatically stored in the database and checked before executing a model.
</aside>


## &bull; NDVI

```shell
X="-1909498"
Y="2978512"
T1="2013-01-01"
T2="2014-01-01"

JOB=$(curl -s \
  -H "$LCMAP_ACCEPT_HDR" \
  -H "$LCMAP_TOKEN_HDR" \
  -X POST "http://localhost:1077/api/models/ndvi?x=$X&y=$Y&t1=$T1&t2=$T2")

echo $JOB
```

```python
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

## CONTINUOUS CHANGE DETECTION AND CLASSIFICATION

Continuous Change Detection and Classification (CCDC) is an algorithm for analyzing land cover using Landsat data. It is capable of detecting many kinds of land cover change continuously as new images are collected and providing land cover maps for any given time.

A two-step cloud, cloud shadow, and snow masking algorithm is used for eliminating "noisy" observations.

A time series model that has components of seasonality, trend, and break estimates surface reflectance and brightness temperature. The time series model is updated dynamically with newly acquired observations.

Due to the differences in spectral response for various kinds of land cover change, the CCDC algorithm uses a threshold derived from all seven Landsat bands. When the difference between observed and predicted images exceeds a threshold six consecutive times, a pixel is identified as land surface change.


## &bull; CCDC Execution

> Generate a CCDC model for the given extents, time-range, and ...:


```shell
TBD
```

```python
>>> from lcmap.client import Client
>>> client = Client()
>>> response = client.models.ccdc.run(
        spectra="", x_val="", y_val="", start_time="", end_time="")
>>> response.result["link"]["href"]
u'/api/jobs/ccdc/piped-processes/1d674fd3b6d7973fbb2cdc9d1cd7f012'
```

```vb
TBD
```

```clojure
TBD
```

```ruby
TBD
```

```r
TBD
```

The CCDC is currenly called with the folowing parameters:
 * Spectra
 * Point x-value
 * Point y-value
 * Start time
 * End time


## &bull; CCDC Results Storage

TBD


## &bull; CCDC Prediction

TBD
