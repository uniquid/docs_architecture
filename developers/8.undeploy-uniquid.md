# Undeploy UniquID

If you have completed evaluating the AWS IoT integration, it is possible to delete all of the AWS resources which were created in your AWS account. Run the following command to undeploy the AWS integration components:

```bash
sudo uniquid undeploy aws <timestamp>
```

The `<timestamp>` field must be a list of one or more timestamps which identify previous runs of the `uniquid deploy aws` command. The timestamp is available in the log file created by the UniquID CLI tool.

