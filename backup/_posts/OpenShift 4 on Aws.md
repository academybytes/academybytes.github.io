# OpenShift 4 on Aws

sign up for OpenShift 4 preview at [try.openshift.org][ocp4]




notes:

- will need to create cluster in a region with NO elastic IPs in use. AWS limits each region to 5 EIPs

````
DEBUG Route found in openshift-console namespace: console 
DEBUG OpenShift console route is created           
INFO Install complete!                            
INFO Run 'export KUBECONFIG=/Users/trilliams/openshift-4/auth/kubeconfig' to manage the cluster with 'oc', the OpenShift CLI. 
INFO The cluster is ready when 'oc login -u kubeadmin -p 4knLR-3Kxzt-xGDTS-BFA6V' succeeds (wait a few minutes). 
INFO Access the OpenShift web-console here: https://console-openshift-console.apps.ocp4.koolbernetes.io 
INFO Login to the console with user: kubeadmin, password: 4knLR-3Kxzt-xGDTS-BFA6V 
````
`openshift-install create cluster` creates bootstrap instance, 3 masters w/EIPs, & 3 "worker" nodes w/o eip.

in the event that you need to start over with a new cluster, delete it with `./openshift-installer destroy cluster`, THEN remove these files from your working directory:

- `metadata.json`
- `terraform.tfvars`


default ssh username for instances is `core`


{"auths":{"cloud.openshift.com":{"auth":"b3BlbnNoaWZ0LXJlbGVhc2UtZGV2K3RyaWJlY2N1czFlYjJtdDhnbnNsbHhxc29xZGpvcHUwMWRuZzo0TVRZSUlCMlpBSjExREdBMUY1QzFTVU1GVTBMNDlQMFMwU0YwWllHVDRQNkRNMEpDWTExNDRNVTFBQlBFTjkw","email":"tribecca@tribecc.us"},"quay.io":{"auth":"b3BlbnNoaWZ0LXJlbGVhc2UtZGV2K3RyaWJlY2N1czFlYjJtdDhnbnNsbHhxc29xZGpvcHUwMWRuZzo0TVRZSUlCMlpBSjExREdBMUY1QzFTVU1GVTBMNDlQMFMwU0YwWllHVDRQNkRNMEpDWTExNDRNVTFBQlBFTjkw","email":"tribecca@tribecc.us"}}}


oc v4.0 

````
[root@ip-10-0-8-6 ~]# oc version
oc v4.0.0-alpha.0+85a0623-808
kubernetes v1.11.0+85a0623
features: Basic-Auth GSSAPI Kerberos SPNEGO
````
````
oc -h
OpenShift Client 

This client helps you develop, build, deploy, and run your applications on any OpenShift or Kubernetes compatible
platform. It also includes the administrative commands for managing a cluster under the 'adm' subcommand.

Usage:
  oc [flags]

Basic Commands:
  login           Log in to a server
  new-project     Request a new project
  new-app         Create a new application
  status          Show an overview of the current project
  project         Switch to another project
  projects        Display existing projects
  explain         Documentation of resources
  cluster-info    Display cluster info

Build and Deploy Commands:
  rollout         Manage a Kubernetes deployment or OpenShift deployment config
  rollback        Revert part of an application back to a previous deployment
  new-build       Create a new build configuration
  start-build     Start a new build
  cancel-build    Cancel running, pending, or new builds
  import-image    Imports images from a Docker registry
  tag             Tag existing images into image streams

Application Management Commands:
  get             Display one or many resources
  describe        Show details of a specific resource or group of resources
  edit            Edit a resource on the server
  set             Commands that help set specific features on objects
  label           Update the labels on a resource
  annotate        Update the annotations on a resource
  expose          Expose a replicated application as a service or route
  delete          Delete one or more resources
  scale           Change the number of pods in a deployment
  autoscale       Autoscale a deployment config, deployment, replication controller, or replica set
  secrets         Manage secrets
  serviceaccounts Manage service accounts in your project

Troubleshooting and Debugging Commands:
  logs            Print the logs for a resource
  rsh             Start a shell session in a pod
  rsync           Copy files between local filesystem and a pod
  port-forward    Forward one or more local ports to a pod
  debug           Launch a new instance of a pod for debugging
  exec            Execute a command in a container
  proxy           Run a proxy to the Kubernetes API server
  attach          Attach to a running container
  run             Run a particular image on the cluster
  cp              Copy files and directories to and from containers.
  wait            Experimental: Wait for one condition on one or many resources

Advanced Commands:
  adm             Tools for managing a cluster
  create          Create a resource from a file or from stdin.
  replace         Replace a resource by filename or stdin
  apply           Apply a configuration to a resource by filename or stdin
  patch           Update field(s) of a resource using strategic merge patch
  process         Process a template into list of resources
  extract         Extract secrets or config maps to disk
  idle            Idle scalable resources
  observe         Observe changes to resources and react to them (experimental)
  policy          Manage authorization policy
  auth            Inspect authorization
  convert         Convert config files between different API versions
  import          Commands that import applications
  image           Useful commands for managing images
  registry        Commands for working with the registry
  api-versions    Print the supported API versions on the server, in the form of "group/version"
  api-resources   Print the supported API resources on the server

Settings Commands:
  logout          End the current server session
  config          Change configuration files for the client
  whoami          Return information about the current session
  completion      Output shell completion code for the specified shell (bash or zsh)

Other Commands:
  ex              Experimental commands under active development
  help            Help about any command
  plugin          Runs a command-line plugin
  version         Display client and server versions

Use "oc <command> --help" for more information about a given command.
Use "oc options" for a list of global command-line options (applies to all commands).
````

[ocp4]: https://try.openshift.com