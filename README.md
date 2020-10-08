# Tanzu Build Service Update Pipeline

The purpose of this pipeline is to monitor TanzuNet for buildpack and stack updates and install them as they become available.

## Maintainer

- Chris Willis (mailto:chwillis@vmware.com)

## TO DO
- Create our own regularly-updated tools container image so that we arent always installing kp and docker CLI

## Set Variables
The following variables are required in credhub or `vars.yml`.

| Keyword       | Explanation                                                                            |
|:--------------|:---------------------------------------------------------------------------------------|
| `harbor_hostname` | the Harbor instance TBS has been configured to use |
| `harbor_username` | a username that has access to the Harbor project TBS uses |
| `harbor_password` | password for `harbord_username` |
| `harbor_project` | the name of the Harbor project that TBS uses |
| `kubeconfig` | the full kubeconfig for a service account that has appropriate access to the cluster running TBS |
| `pivnet_api_token` | API token for a pivnet account that will be used to retrieve TBS updates |
| `pivnet_username` | username for a pivnet account that will be used to retrieve TBS updates |
| `pivnet_password` | password for a pivnet account that will be used to retrieve TBS updates |
| `ca_cert` | CA cert that Harbor uses |

## Getting Started
0. `fly -t dev login -c https://<CONCOURSE_URL>`
0. Edit `vars.yml` and put in the appropriate values
0. `fly -t dev sp -p "Update TBS <ENV_NAME>" -c ./pipeline.yml -l ./vars.yml`
0. `fly -t dev up -p "Update TBS <ENV_NAME>"`
0. `fly -t dev tj -j "Update TBS <ENV_NAME>/kp-import"`

### Notes
* To delete a previous tanzu build service install run `kapp delete -a tanzu-build-service`
* Make sure to clean up `kp secret list` prior to running pipeline or the `kp create secret` steps may fail in the pipeline
