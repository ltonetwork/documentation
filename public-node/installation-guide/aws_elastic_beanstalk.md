# AWS Elastic Beanstalk

### Run in AWS Elastic Beanstalk

Running the node using AWS Elastic Beanstalk \(EB\) it will only install the services on a machine. This node includes a Redis database, however it is highly recommended to use AWS Elastic Cache. The are 2 EB configuration files included.

1. Dockerrun.elastic-cache.aws.json
2. Dockerrun.redis.aws.json

Take to following steps to install the node on EB:

1. Choose if you wish to run the node with or without redis. Rename the correct config file to Dockerrun.aws.json
2. Zip the Dockerrun.aws.json file
3. Create an application
4. Inside the created application, create an environment: `webserver environment`
5. Select following settings:
   * Platform: Multi-container Docker
   * Upload the zipped file
6. Configure more options
7. Instances -&gt; Instance type: Choose an instance with atleast 2 gb of memory \(E.g. t2.small\)
8. Software -&gt; Environment properties:
   * Name: `LTO_WALLET_PASSWORD`, Value: `Your wallet password`
   * Name: `LTO_WALLET_SEED` or `LTO_WALLET_SEED_BASE58`, Value: `Wallet Seed`
   * Name: `ANCHOR_REDIS_URL`, Value: `"<Your redis connection string>"` \(If you are running with elastic cache\)

Now your node is should good to go!

