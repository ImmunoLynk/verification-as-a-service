# ImmunoLynk: Verification-as-a-Service
  
Immunity test recognition, verification and distribution service compatible with SingularityNET.

## How it works 

Users/developers can leverage our computer vision model to verify pictures of laminar flow immunity tests for COVID-19 (see result_recognition/data for examples).

In addition to that, users can opt to store the result in our public (free) IPFS node, donating their anonymized data to better help health officials/researchers/innovators on data-driven solutions for the pandemic crisis.

Alternatively, users can opt into the premium service of hosting a private IPFS node so they can futurely commercialize their data through Ocean Protocol.

We still recommend using our app since it also has the immunity questionaire, which can also be commercialized through Ocean. I also complements the test image pretty nicely, since we can approximate an Immunity Score to assist partial result recommendation.

## Getting Started

### Prerequisites

* [Python 3.6.5](https://www.python.org/downloads/release/python-365/)
* Tensorflow 
* Keras

### Installing

* Clone the git repository:

```
git clone https://github.com/singnet/example-service.git
cd example-service
```

* Install the dependencies and compile the protobuf file:

```
pip3 install -r requirements.txt
sh buildproto.sh
```

### Running

#### With SingularityNET Daemon

##### SingularityNET Daemon Configuration

To get the `ORGANIZATION_ID` and `SERVICE_ID` you must have already published a service 
(check this [link](https://dev.singularitynet.io/tutorials/publish/)).

Create the `SNET Daemon`'s config JSON file (`snetd.config.json`).

```
{
   "DAEMON_END_POINT": "__DAEMON_HOST__:__DAEMON_PORT__",
   "BLOCKCHAIN_NETWORK_SELECTED": "__NETWORK__",
   "IPFS_END_POINT": "http://ipfs.singularitynet.io:80",
   "PASSTHROUGH_ENDPOINT": "http://__SERVICE_GRPC_HOST__:__SERVICE_GRPC_PORT__",
   "ORGANIZATION_ID": "__ORGANIZATION_ID__",
   "SERVICE_ID": "__SERVICE_ID__",
   "PAYMENT_CHANNEL_STORAGE_SERVER": {
       "DATA_DIR": "/opt/singnet/etcd/__NETWORK__"
   },
   "LOG": {
       "LEVEL": "debug",
       "OUTPUT": {
            "TYPE": "stdout"
           }
   }
}
```

For example, using the Ropsten testnet, replace tags with:

- `__DAEMON_HOST__:__DAEMON_PORT__`: localhost:7000
- `__NETWORK__`: ropsten (main for Mainnet)
- `http://__SERVICE_GRPC_HOST__:__SERVICE_GRPC_PORT__`: http://localhost:7003
- `__ORGANIZATION_ID__`: my-organization
- `__SERVICE_ID__`: my-service

See [SingularityNet daemon configuration](https://github.com/singnet/snet-daemon/blob/master/README.md#configuration) for detailed configuration description.

##### Running Service + Daemon in a Docker Container

* Build the docker image and run a container from it:

```
docker build \
    -t snet_example_service \
    https://github.com/singnet/example-service.git#master

export ETCD_HOST=$HOME/.snet/etcd/example-service/
export ETCD_CONTAINER=/opt/singnet/etcd/
docker run \
    -p 7000:7000 \
    -v $ETCD_HOST:$ETCD_CONTAINER \
    -ti snet_example_service bash
```

Note that the `$ETCD_(HOST|CONTAINER)` are useful to keep your service's etcd folder outside the container.

From this point we follow the tutorial in the Docker Container's prompt.

After this, run the service (with `SNET Daemon`), make sure you have the `snetd.config.json` file in the service folder:

```
# cat snetd.config.json
{
   "DAEMON_END_POINT": "0.0.0.0:7000",
   "BLOCKCHAIN_NETWORK_SELECTED": "ropsten",
   "IPFS_END_POINT": "http://ipfs.singularitynet.io:80",
   "PASSTHROUGH_ENDPOINT": "http://localhost:7003",
   "ORGANIZATION_ID": "my-organization",
   "SERVICE_ID": "my-service",
   "PAYMENT_CHANNEL_STORAGE_SERVER": {
       "DATA_DIR": "/opt/singnet/etcd/ropsten"
   },
   "LOG": {
       "LEVEL": "debug",
       "OUTPUT": {
            "TYPE": "stdout"
           }
   }
}
# python3 run_example_service.py --daemon-config snetd.config.json &
```

## License

This project is licensed under the MIT License - see the
[LICENSE](https://github.com/singnet/example-service/blob/master/LICENSE) file for details.
