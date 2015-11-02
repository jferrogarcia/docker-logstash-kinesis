# Overview

based on logstash 1.5.4 image, with kinesis plugin support - see `logstash-output-kinesis`

# Configuration

Create a `logstash.conf` file following logstash configuration files format. Add any input that you wish to and add the kinesis output as follow -let's imagine you want to tail a file in json format and send the content to kinesis:

```
input {
  file {
    path => "/config-dir/your-fancy-log-file.log"
    codec => json {}
    start_position => "beginning"
  }
}
filter {
}
output {
  kinesis {
    metrics_level => "none"
    access_key => "@{your_aws_access_key}"
    secret_key => "@{your_aws_secret_key}"
    stream_name => "logs"
    region => "us-west-1"
    event_partition_keys => [@{your_partitioning_keys}]
  }
}
```

# Run

Mount the volumes where your conf files and logs files are. Please, you need to be sure that conf file points to a folder that can be reached from your docker image:

```
docker run -it -v "$PWD":/config-dir jferrogarcia/logstash-kinesis logstash -f /config-dir/@{your_logstash_conf_file}
```
