# <img src="https://github.com/mostafa/xk6-kafka/blob/main/assets/xk6-kafka-logo.png" alt="xk6-kafka logo" style="height: 32px; width:32px;"/> xk6-kafka

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/mostafa/xk6-kafka/test.yaml?branch=main&logo=github)](https://github.com/mostafa/xk6-kafka/actions) [![Docker Pulls](https://img.shields.io/docker/pulls/mostafamoradian/xk6-kafka?logo=docker)](https://hub.docker.com/r/mostafamoradian/xk6-kafka) [![Coverage Status](https://coveralls.io/repos/github/mostafa/xk6-kafka/badge.svg?branch=main)](https://coveralls.io/github/mostafa/xk6-kafka?branch=main) [![Go Reference](https://pkg.go.dev/badge/github.com/mostafa/xk6-kafka.svg)](https://pkg.go.dev/github.com/mostafa/xk6-kafka)

The xk6-kafka project is a [k6 extension](https://k6.io/docs/extensions/guides/what-are-k6-extensions/) that enables k6 users to load test Apache Kafka using a producer and possibly a consumer for debugging.

The real purpose of this extension is to test the system you meticulously designed to use Apache Kafka. So, you can test your consumers, hence your system, by auto-generating messages and sending them to your system via Apache Kafka.

You can send many messages with each connection to Kafka. These messages are arrays of objects containing a key and a value in various serialization formats, passed via configuration objects. Various serialization formats are supported, including strings, JSON, binary, Avro, and JSON Schema. Avro and JSON Schema can either be fetched from Schema Registry or hard-code directly in the script. SASL PLAIN/SCRAM authentication and message compression are also supported.

For debugging and testing purposes, a consumer is available to make sure you send the correct data to Kafka.

If you want to learn more about the extension, read the [article](https://k6.io/blog/load-test-your-kafka-producers-and-consumers-using-k6/) (outdated) explaining how to load test your Kafka producers and consumers using k6 on the k6 blog. You can also watch [this recording](https://www.youtube.com/watch?v=NQ0fyhq1mxo) of the k6 Office Hours about this extension.

## Supported Features

- Produce/consume messages as [String](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_string.js), [JSON](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_json.js), [ByteArray](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_bytes.js), [Avro](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_avro_with_schema_registry.js) and [JSON Schema](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_jsonschema_with_schema_registry.js) formats
- Support for user-provided [Avro](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_avro_no_schema_registry.js) and JSON Schema key and value schemas in the script
- Authentication with [SASL PLAIN, SCRAM and SSL](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_sasl_auth.js)
- Create, list and delete [topics](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_topics.js)
- Support for loading Avro schemas from [Schema Registry](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_avro_with_schema_registry.js)
- Support for [byte array](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_bytes.js) for binary data (from binary protocols)
- Support consumption from all partitions with a [group ID](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_consumer_group.js)
- Support Kafka message compression: Gzip, [Snappy](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_json.js), Lz4 & Zstd
- Support for sending messages with [no key](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_avro_no_schema_registry.js)
- Support for k6 [thresholds](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_json.js) on custom Kafka metrics
- Support for [headers](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_json.js) on produced and consumed messages
- Lots of exported metrics, as shown in the result output of the [k6 test script](https://github.com/mostafa/xk6-kafka/blob/main/README.md#k6-test-scripts)

## Download Binaries

### The Official Docker Image

The [official Docker image](https://hub.docker.com/r/mostafamoradian/xk6-kafka) is available on Docker Hub. Before running your script, make the script available to the container by mounting a volume (a directory) or passing it via stdin.

```bash
docker run --rm -i mostafamoradian/xk6-kafka:latest run - <scripts/test_json.js
```

### The Official Binaries

The binaries are generated by the build process and published on the [releases page](https://github.com/mostafa/xk6-kafka/releases). Currently, binaries for the GNU/Linux, macOS, and Windows on `amd64` (`x86_64`) machines are available.

> **Note:**
> If you want to see an official build for your machine, please build and test xk6-kafka from [source](https://github.com/mostafa/xk6-kafka/blob/main/README.md#build-from-source) and then create an [issue](https://github.com/mostafa/xk6-kafka/issues/new) with details. I'll add the specific binary to the build pipeline and publish them on the next release.

## Build from Source

You can build the k6 binary on various platforms, each with its requirements. The following shows how to build k6 binary with this extension on GNU/Linux distributions.

### Prerequisites

You must have the latest Go version installed to build the k6 binary. The latest version should match [k6](https://github.com/grafana/k6#build-from-source) and [xk6](https://github.com/grafana/xk6#requirements). I recommend [gvm](https://github.com/moovweb/gvm) because it eases version management.

- [gvm](https://github.com/moovweb/gvm) for easier installation and management of Go versions on your machine
- [Git](https://git-scm.com/) for cloning the project
- [xk6](https://github.com/grafana/xk6) for building k6 binary with extensions

### Install and build the latest tagged version

Feel free to skip the first two steps if you already have Go installed.

1. Install gvm by following its [installation guide](https://github.com/moovweb/gvm#installing).
2. Install the latest version of Go using gvm. You need Go 1.4 installed for bootstrapping into higher Go versions, as explained [here](https://github.com/moovweb/gvm#a-note-on-compiling-go-15).
3. Install `xk6`:

   ```shell
   go install go.k6.io/xk6/cmd/xk6@latest
   ```

4. Build the binary:

   ```shell
   xk6 build --with github.com/mostafa/xk6-kafka@latest
   ```

> **Note**
> You can always use the latest version of k6 to build the extension, but the earliest version of k6 that supports extensions via xk6 is v0.32.0. The xk6 is constantly evolving, so some APIs may not be backward compatible.

### Build for development

If you want to add a feature or make a fix, clone the project and build it using the following commands. The xk6 will force the build to use the local clone instead of fetching the latest version from the repository. This process enables you to update the code and test it locally.

```bash
git clone git@github.com:mostafa/xk6-kafka.git && cd xk6-kafka
xk6 build --with github.com/mostafa/xk6-kafka@latest=.
```

## Example scripts

There are many examples in the [script](https://github.com/mostafa/xk6-kafka/blob/main/scripts/) directory that show how to use various features of the extension.

## How to Test You Kafka Setup

You can start testing your setup immediately, but it takes some time to develop the script, so it would be better to test your script against a development environment and then start testing your environment.

### Development environment

I recommend the [fast-data-dev](https://github.com/lensesio/fast-data-dev) Docker image by Lenses.io, a Kafka setup for development that includes Kafka, Zookeeper, Schema Registry, Kafka-Connect, Landoop Tools, 20+ connectors. It is relatively easy to set up if you have Docker installed. Just monitor Docker logs to have a working setup before attempting to test because the initial setup, leader election, and test data ingestion take time.

1. Run the Kafka environment and expose the ports:

   ```bash
   sudo docker run \
       --detach --rm \
       --name lensesio \
       -p 2181:2181 \
       -p 3030:3030 \
       -p 8081-8083:8081-8083 \
       -p 9581-9585:9581-9585 \
       -p 9092:9092 \
       -e ADV_HOST=127.0.0.1 \
       -e RUN_TESTS=0 \
       lensesio/fast-data-dev:latest
   ```

2. After running the command, visit [localhost:3030](http://localhost:3030) to get into the fast-data-dev environment.

3. You can run the command to see the container logs:

   ```bash
   sudo docker logs -f -t lensesio
   ```

> **Note:**
> If you have errors running the Kafka development environment, refer to the [fast-data-dev documentation](https://github.com/lensesio/fast-data-dev).

### The xk6-kafka API

All the exported functions are available by importing the module object from `k6/x/kafka`. The exported objects, constants and other data structures are available in the [`index.d.ts`](https://github.com/mostafa/xk6-kafka/blob/main/api-docs/index.d.ts) file, and they always reflect the _latest_ changes on the `main` branch. You can access the generated documentation at [`api-docs/docs/README.md`](https://github.com/mostafa/xk6-kafka/blob/main/api-docs/docs/README.md).

> ⚠️ **Warning:**
> The Javascript API is subject to change in future versions unless a new major version is released.

### k6 Test Scripts

The example scripts are available as `test_<format/feature>.js` with more code and commented sections in the [scripts](https://github.com/mostafa/xk6-kafka/blob/main/scripts/) directory. Since this project extends the functionality of k6, it has four stages in the [test life cycle](https://k6.io/docs/using-k6/test-life-cycle/).

1. To use the extension, you need to import it in your script, like any other JS module:

   ```javascript
   // Either import the module object
   import * as kafka from "k6/x/kafka";

   // Or individual classes and constants
   import {
     Writer,
     Reader,
     Connection,
     SchemaRegistry,
     SCHEMA_TYPE_STRING,
   } from "k6/x/kafka";
   ```

2. You need to instantiate the classes in the `init` context. All the [k6 options](https://k6.io/docs/using-k6/k6-options/) are also configured here:

   ```javascript
   // Creates a new Writer object to produce messages to Kafka
   const writer = new Writer({
     // WriterConfig object
     brokers: ["localhost:9092"],
     topic: "my-topic",
   });

   const reader = new Reader({
     // ReaderConfig object
     brokers: ["localhost:9092"],
     topic: "my-topic",
   });

   const connection = new Connection({
     // ConnectionConfig object
     address: "localhost:9092",
   });

   const schemaRegistry = new SchemaRegistry();
   // Can accept a SchemaRegistryConfig object

   if (__VU == 0) {
     // Create a topic on initialization (before producing messages)
     connection.createTopic({
       // TopicConfig object
       topic: "my-topic",
     });
   }
   ```

3. In the VU code, you can produce messages to Kafka or consume messages from it:

   ```javascript
   export default function () {
     // Fetch the list of all topics
     const topics = connection.listTopics();
     console.log(topics); // list of topics

     // Produces message to Kafka
     writer.produce({
       // ProduceConfig object
       messages: [
         // Message object(s)
         {
           key: schemaRegistry.serialize({
             data: "my-key",
             schemaType: SCHEMA_TYPE_STRING,
           }),
           value: schemaRegistry.serialize({
             data: "my-value",
             schemaType: SCHEMA_TYPE_STRING,
           }),
         },
       ],
     });

     // Consume messages from Kafka
     let messages = reader.consume({
       // ConsumeConfig object
       limit: 10,
     });

     // your messages
     console.log(messages);

     // You can use checks to verify the contents,
     // length and other properties of the message(s)

     // To serialize the data back into a string, you should use
     // the deserialize method of the Schema Registry client. You
     // can use it inside a check, as shown in the example scripts.
     let deserializedValue = schemaRegistry.deserialize({
       data: messages[0].value,
       schemaType: SCHEMA_TYPE_STRING,
     });
   }
   ```

4. In the `teardown` function, close all the connections and possibly delete the topic:

   ```javascript
   export function teardown(data) {
     // Delete the topic
     connection.deleteTopic("my-topic");

     // Close all connections
     writer.close();
     reader.close();
     connection.close();
   }
   ```

5. You can now run k6 with the extension using the following command:

   ```bash
   ./k6 run --vus 50 --duration 60s scripts/test_json.js
   ```

6. And here's the test result output:

   ```bash

           /\      |‾‾| /‾‾/   /‾‾/
      /\  /  \     |  |/  /   /  /
     /  \/    \    |     (   /   ‾‾\
    /          \   |  |\  \ |  (‾)  |
   / __________ \  |__| \__\ \_____/ .io

   execution: local
       script: scripts/test_json.js
       output: -

   scenarios: (100.00%) 1 scenario, 50 max VUs, 1m30s max duration (incl. graceful stop):
           * default: 50 looping VUs for 1m0s (gracefulStop: 30s)


   running (1m04.4s), 00/50 VUs, 20170 complete and 0 interrupted iterations
   default ✓ [======================================] 50 VUs  1m0s

       ✓ 10 messages are received
       ✓ Topic equals to xk6_kafka_json_topic
       ✓ Key contains key/value and is JSON
       ✓ Value contains key/value and is JSON
       ✓ Header equals {'mykey': 'myvalue'}
       ✓ Time is past
       ✓ Partition is zero
       ✓ Offset is gte zero
       ✓ High watermark is gte zero

       █ teardown

       checks.........................: 100.00% ✓ 181530       ✗ 0
       data_received..................: 0 B     0 B/s
       data_sent......................: 0 B     0 B/s
       iteration_duration.............: avg=153.45ms min=6.01ms med=26.8ms  max=8.14s   p(90)=156.3ms p(95)=206.4ms
       iterations.....................: 20170   313.068545/s
       kafka_reader_dial_count........: 50      0.776075/s
       kafka_reader_dial_seconds......: avg=171.22µs min=0s     med=0s      max=1.09s   p(90)=0s      p(95)=0s
     ✓ kafka_reader_error_count.......: 0       0/s
       kafka_reader_fetch_bytes_max...: 1000000 min=1000000    max=1000000
       kafka_reader_fetch_bytes_min...: 1       min=1          max=1
       kafka_reader_fetch_wait_max....: 200ms   min=200ms      max=200ms
       kafka_reader_fetch_bytes.......: 58 MB   897 kB/s
       kafka_reader_fetch_size........: 147167  2284.25179/s
       kafka_reader_fetches_count.....: 107     1.6608/s
       kafka_reader_lag...............: 1519055 min=0          max=2436190
       kafka_reader_message_bytes.....: 40 MB   615 kB/s
       kafka_reader_message_count.....: 201749  3131.446006/s
       kafka_reader_offset............: 4130    min=11         max=5130
       kafka_reader_queue_capacity....: 1       min=1          max=1
       kafka_reader_queue_length......: 1       min=0          max=1
       kafka_reader_read_seconds......: avg=96.5ms   min=0s     med=0s      max=59.37s  p(90)=0s      p(95)=0s
       kafka_reader_rebalance_count...: 0       0/s
       kafka_reader_timeouts_count....: 57      0.884725/s
       kafka_reader_wait_seconds......: avg=102.71µs min=0s     med=0s      max=85.71ms p(90)=0s      p(95)=0s
       kafka_writer_acks_required.....: 0       min=0          max=0
       kafka_writer_async.............: 0.00%   ✓ 0            ✗ 2017000
       kafka_writer_attempts_max......: 0       min=0          max=0
       kafka_writer_batch_bytes.......: 441 MB  6.8 MB/s
       kafka_writer_batch_max.........: 1       min=1          max=1
       kafka_writer_batch_size........: 2017000 31306.854525/s
       kafka_writer_batch_timeout.....: 0s      min=0s         max=0s
     ✓ kafka_writer_error_count.......: 0       0/s
       kafka_writer_message_bytes.....: 883 MB  14 MB/s
       kafka_writer_message_count.....: 4034000 62613.709051/s
       kafka_writer_read_timeout......: 0s      min=0s         max=0s
       kafka_writer_retries_count.....: 0       0/s
       kafka_writer_wait_seconds......: avg=0s       min=0s     med=0s      max=0s      p(90)=0s      p(95)=0s
       kafka_writer_write_count.......: 4034000 62613.709051/s
       kafka_writer_write_seconds.....: avg=523.21µs min=4.84µs med=14.48µs max=4.05s   p(90)=33.85µs p(95)=42.68µs
       kafka_writer_write_timeout.....: 0s      min=0s         max=0s
       vus............................: 7       min=7          max=50
       vus_max........................: 50      min=50         max=50
   ```

### Emitted Metrics

| Metric                       | Type    | Description                                                             |
| ---------------------------- | ------- | ----------------------------------------------------------------------- |
| kafka_reader_dial_count      | Counter | Total number of times the reader tries to connect.                      |
| kafka_reader_fetches_count   | Counter | Total number of times the reader fetches batches of messages.           |
| kafka_reader_message_count   | Counter | Total number of messages consumed.                                      |
| kafka_reader_message_bytes   | Counter | Total bytes consumed.                                                   |
| kafka_reader_rebalance_count | Counter | Total number of rebalances of a topic in a consumer group (deprecated). |
| kafka_reader_timeouts_count  | Counter | Total number of timeouts occurred when reading.                         |
| kafka_reader_error_count     | Counter | Total number of errors occurred when reading.                           |
| kafka_reader_dial_seconds    | Trend   | The time it takes to connect to the leader in a Kafka cluster.          |
| kafka_reader_read_seconds    | Trend   | The time it takes to read a batch of message.                           |
| kafka_reader_wait_seconds    | Trend   | Waiting time before read a batch of messages.                           |
| kafka_reader_fetch_size      | Counter | Total messages fetched.                                                 |
| kafka_reader_fetch_bytes     | Counter | Total bytes fetched.                                                    |
| kafka_reader_offset          | Gauge   | Number of messages read after the given offset in a batch.              |
| kafka_reader_lag             | Gauge   | The lag between the last message offset and the current read offset.    |
| kafka_reader_fetch_bytes_min | Gauge   | Minimum number of bytes fetched.                                        |
| kafka_reader_fetch_bytes_max | Gauge   | Maximum number of bytes fetched.                                        |
| kafka_reader_fetch_wait_max  | Gauge   | The maximum time it takes to fetch a batch of messages.                 |
| kafka_reader_queue_length    | Gauge   | The queue length while reading batch of messages.                       |
| kafka_reader_queue_capacity  | Gauge   | The queue capacity while reading batch of messages.                     |
| kafka_writer_write_count     | Counter | Total number of times the writer writes batches of messages.            |
| kafka_writer_message_count   | Counter | Total number of messages produced.                                      |
| kafka_writer_message_bytes   | Counter | Total bytes produced.                                                   |
| kafka_writer_error_count     | Counter | Total number of errors occurred when writing.                           |
| kafka_writer_write_seconds   | Trend   | The time it takes writing messages.                                     |
| kafka_writer_wait_seconds    | Trend   | Waiting time before writing messages.                                   |
| kafka_writer_retries_count   | Counter | Total number of attempts at writing messages.                           |
| kafka_writer_batch_size      | Counter | Total batch size.                                                       |
| kafka_writer_batch_bytes     | Counter | Total number of bytes in a batch of messages.                           |
| kafka_writer_attempts_max    | Gauge   | Maximum number of attempts at writing messages.                         |
| kafka_writer_batch_max       | Gauge   | Maximum batch size.                                                     |
| kafka_writer_batch_timeout   | Gauge   | Batch timeout.                                                          |
| kafka_writer_read_timeout    | Gauge   | Batch read timeout.                                                     |
| kafka_writer_write_timeout   | Gauge   | Batch write timeout.                                                    |
| kafka_writer_acks_required   | Gauge   | Required Acks.                                                          |
| kafka_writer_async           | Rate    | Async writer.                                                           |

### FAQ

1. Why do I receive `Error writing messages`?

   There are a few reasons why this might happen. The most prominent one is that the topic might not exist, which causes the producer to fail to send messages to a non-existent topic. You can use `Connection.createTopic` method to create the topic in Kafka, as shown in `scripts/test_topics.js`. You can also set the `autoCreateTopic` on the `WriterConfig`. You can also create a topic using the `kafka-topics` command:

   ```bash
   $ docker exec -it lensesio bash
   (inside container)$ kafka-topics --create --topic xk6_kafka_avro_topic --bootstrap-server localhost:9092
   (inside container)$ kafka-topics --create --topic xk6_kafka_json_topic --bootstrap-server localhost:9092
   ```

2. Why does the `reader.consume` keep hanging?

   If the `reader.consume` keeps hanging, it might be because the topic doesn't exist or is empty.

3. I want to test SASL authentication. How should I do that?

   If you want to test SASL authentication, look at [this commit message](https://github.com/mostafa/xk6-kafka/pull/3/commits/403fbc48d13683d836b8033eeeefa48bf2f25c6e), in which I describe how to run a test environment to test SASL authentication.

4. Why doesn't the consumer group consume messages from the topic?

   As explained in issue [#37](https://github.com/mostafa/xk6-kafka/issues/37), multiple inits by k6 cause multiple consumer group instances to be created in the init context, which sometimes causes the random partitions to be selected by each instance. This, in turn, causes confusion when consuming messages from different partitions. This can be solved by using a UUID when naming the consumer group, thereby guaranteeing that the consumer group object was assigned to all partitions in a topic.

5. Why do I receive a `MessageTooLargeError` when I produce messages bigger than 1 MB?

   Kafka has a [maximum message size](https://docs.confluent.io/platform/current/installation/configuration/broker-configs.html) of 1 MB by default, which is set by `message.max.bytes`, and this limit is also applied to the `Writer` object.

   There are two ways to produce larger messages: 1) Change the default value of your Kafka instance to a larger number. 2) Use compression.

   Remember that the `Writer` object will reject messages larger than the default Kafka message size limit (1 MB). Hence you need to set `batchBytes` to a larger value, for example, `1024 * 1024 * 2` (2 MB). The `batchBytes` refers to the raw uncompressed size of all the keys and values (data) in your array of messages you pass to the `Writer` object. You can calculate the raw data size of your messages using [this example script](https://github.com/mostafa/xk6-kafka/issues/181#issuecomment-1325390880).

6. Can I consume messages from a consumer group in a topic with multiple partitions?

   Yes, you can. Just pass the `groupID` to your `Reader` object. You must not specify the partition anymore. Visit this [documentation article](https://docs.confluent.io/platform/current/clients/consumer.html#concepts) to learn more about Kafka consumer groups.

   Remember that you must set `sessionTimeout` on your `Reader` object if the consume function terminates abruptly, thus failing to consume messages.

7. Why does the `Reader.consume` produces an `unable to read message` error?

   For performance testing reasons, the `maxWait` of the `Reader` is set to 200ms. If you keep receiving this error, consider increasing it to a larger value.

8. How can I consume from multiple partitions on a single topic?

   You can configure your reader to consume from a (list of) topic(s) and its partitions using a consumer group. This can be achieve by setting `groupTopics`, `groupID` and a few other options for timeouts, intervals and lags. Have a look at the [`test_consumer_group.js`](https://github.com/mostafa/xk6-kafka/blob/main/scripts/test_consumer_group.js) example script.

## Contributions, Issues and Feedback

I'd be thrilled to receive contributions and feedback on this project. You're always welcome to create an issue if you find one (or many). I would do my best to address the issues. Also, feel free to contribute by opening a PR with changes, and I'll do my best to review and merge it as soon as I can.

## Backward Compatibility Notice

If you want to keep up to date with the latest changes, please follow the [project board](https://github.com/users/mostafa/projects/1). Also, since [v0.9.0](https://github.com/mostafa/xk6-kafka/releases/tag/v0.9.0), the `main` branch is the _development_ branch and usually has the latest changes and might be unstable. If you want to use the latest features, you might need to build your binary by following the [build from source](https://github.com/mostafa/xk6-kafka/blob/main/README.md#build-from-source) instructions. In turn, the tagged releases and the Docker images are more stable.

I make no guarantee to keep the API stable, as this project is in _active development_ unless I release a major version. The best way to keep up with the changes is to follow [the xk6-kafka API](https://github.com/mostafa/xk6-kafka/blob/main/README.md#the-xk6-kafka-api) and look at the [scripts](https://github.com/mostafa/xk6-kafka/blob/main/scripts/) directory.

## The Release Process

The `main` branch is the _development_ branch, and the pull requests will be _squashed and merged_ into the `main` branch. When a commit is tagged with a version, for example, `v0.10.0`, the build pipeline will build the `main` branch on that commit. The build process creates the binaries and the Docker image. If you want to test the latest unreleased features, you can clone the `main` branch and instruct the `xk6` to use the locally cloned repository instead of using the `@latest`, which refers to the latest tagged version, as explained in the [build for development](https://github.com/mostafa/xk6-kafka/blob/main/README.md#build-for-development) section.

## The CycloneDX SBOM

CycloneDX SBOMs in JSON format are generated for [go.mod](go.mod) (as of [v0.9.0](https://github.com/mostafa/xk6-kafka/releases/tag/v0.9.0)) and the Docker image (as of [v0.14.0](https://github.com/mostafa/xk6-kafka/releases/tag/v0.14.0)) and they can be accessed from the the release [assets](https://github.com/mostafa/xk6-kafka/releases/tag/v0.11.0).

## Disclaimer

This project _was_ a proof of concept but seems to be used by some companies nowadays. However, it isn't supported by the k6 team, but rather by [me](https://github.com/mostafa) personally, and the APIs may change in the future. USE AT YOUR OWN RISK!

This project was AGPL3-licensed up until 7 October 2021, and then we [relicensed](https://github.com/mostafa/xk6-kafka/pull/25) it under the [Apache License 2.0](https://github.com/mostafa/xk6-kafka/blob/master/LICENSE).
