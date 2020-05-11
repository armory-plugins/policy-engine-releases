# policy-engine-releases

This repository serves as a home for releses of Armory's Policy Engine plugin for Spinnaker. This plugin is currently in Early Access which means it's pre-1.0 and subject to change. 

## About

Armory's Policy Engine plugin provides hook points for policy application within the lifecycle of Spinnaker. Today, you can apply policy in 3 areas:

1. Pipeline save - save a pipeline and verify that it fits within your org's policies.
2. Deployment - prevent malicious or unchecked deployments from making their way into your cloud.
3. Pipeline execution - besides deployments, Spinnaker provides stages for many different actions like baking an AMI or generating a Kubernetes manifest. Apply policy before and after these actions are executed.

Official documentation for this feature can be found on our [documentation site](https://docs.armory.io/spinnaker/policy_engine/#using-the-policy-engine-to-validate-pipeline-configurations).

_At the time of this writing, the Policy Engine plugin is only supported on the 1.20 release of Spinnaker. Support for Armory Spinnaker is forthcoming._

## Installation & Configuration

_It should be noted that the plugins framework for Spinnaker is still in active development so installation instructions may change_.

1. Identify the released version of the Policy Engine plugin you wish to install. Official releases are found [here](https://github.com/armory-plugins/policy-engine-releases/releases).
2. In your Spinnaker configuration, add the following repository & plugin configuration.

    ```yaml
    spinnaker:
      extensibility:
          plugins:
              Armory.PolicyEngine:
                enabled: true
                version: 0.0.8
                extensions:
                  armory.policyEngine:
                    enabled: true
                    config:
                      baseUrl: http://opa.spinnaker:8181/v1
          repositories:
            armory:
              url: https://raw.githubusercontent.com/armory-plugins/policy-engine-releases/master/repositories.json
    ```
    
    This plugin provides policy hooks across 3 services in Spinnaker: Front50, Orca and Clouddriver. If you'd like to enable only part of it, you can add the above block to 1 or more of the `.hal/default/profiles/{serviceName}-local.yaml` configuration files if you're using Halyard to install Spinnaker. 
    
    The above configuration also assumes that you've installed or deployed OPA using Armory's documentation [found here](https://docs.armory.io/spinnaker/policy_engine/#deploying-an-opa-server-for-policy-engine-to-use).

3. On startup, the plugin will be downloaded and installed to the appropriate Spinnaker services.


## Feedback

We'd love to hear about how you're using our Policy Engine plugin! If you have any feedback or questions, just leave us a comment [here](https://feedback.armory.io/feature-requests/p/provisioning-and-compliance-management-storyboard).
