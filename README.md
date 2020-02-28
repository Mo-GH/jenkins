# Jenkins Helm Chart

Jenkins master and agent cluster utilizing the Jenkins Kubernetes plugin

* https://plugins.jenkins.io/kubernetes

Inspired by the awesome work of Carlos Sanchez <mailto:carlos@apache.org>

## Chart Details

This chart will do the following:

* 1 x Jenkins Master with port 8080 exposed on an "LoadBalancer" or "Port Forwarding"
* All using Kubernetes Deployments
* More info: https://hub.helm.sh/charts/stable/jenkins

## Installing the Chart

To install the chart with the release name `concardis-jenkins` with concardis customize values:

```bash
$ helm install concardis-jenkins stable/jenkins --namespace concardis-jenkins -f concardisvalues.yaml
```

To uninstall the chart with the release name `concardis-jenkins` :

```bash
$ helm uninstall concardis-jenkins --namespace concardis-jenkins 
```

To update configration please use Ansible in Ansible-JCasc:

link here: https://bitbucket.concardis.com/users/ghanemm/repos/mo-ghanem-repo/browse/Ansible-JCasc


To upgrade the chart with the release name `concardis-jenkins` with new configrations (not finished):

```bash
$ helm *** concardis-jenkins --namespace concardis-jenkins 
```

## Configuration

The following tables list the configurable parameters of the Jenkins chart and their default values.

### Jenkins Master

| Parameter                         | Description                          | Default                                   |
| --------------------------------- | ------------------------------------ | ----------------------------------------- |
| `checkDeprecation`                | Checks for deprecated values used    | `true`                                 |
| `clusterZone`                     | Override the cluster name for FQDN resolving    | `cluster.local`                |
| `nameOverride`                    | Override the resource name prefix    | `jenkins`                                 |
| `fullnameOverride`                | Override the full resource names     | `jenkins-{release-name}` (or `jenkins` if release-name is `jenkins`) |
| `namespaceOverride`               | Override the deployment namespace    | Not set (`Release.Namespace`)             |
| `master.componentName`            | Jenkins master name                  | `jenkins-master`                          |
| `master.image`                    | Master image name                    | `jenkins/jenkins`                         |
| `master.tag`                      | Master image tag                     | `lts`                                     |
| `master.imagePullPolicy`          | Master image pull policy             | `Always`                                  |
| `master.imagePullSecret`          | Master image pull secret             | Not set                                   |
| `master.numExecutors`             | Set Number of executors              | 0                                         |
| `master.customJenkinsLabels`      | Append Jenkins labels to the master  | `{}`                                      |
| `master.useSecurity`              | Use basic security                   | `true`                                    |
| `master.securityRealm`            | Custom Security Realm                | Not set                                   |
| `master.authorizationStrategy`    | Jenkins XML job config for AuthorizationStrategy | Not set                       |
| `master.deploymentLabels`         | Custom Deployment labels             | Not set                                   |
| `master.serviceLabels`            | Custom Service labels                | Not set                                   |
| `master.podLabels`                | Custom Pod labels                    | Not set                                   |
| `master.adminUser`                | Admin username (and password) created as a secret if useSecurity is true | `admin` |
| `master.adminPassword`            | Admin password (and user) created as a secret if useSecurity is true | Random value |
| `master.jenkinsAdminEmail`        | Email address for the administrator of the Jenkins instance | Not set            |
| `master.resources`                | Resources allocation (Requests and Limits) | `{requests: {cpu: 50m, memory: 256Mi}, limits: {cpu: 2000m, memory: 4096Mi}}`|
| `master.initContainerEnv`         | Environment variables for Init Container                                 | Not set |
| `master.containerEnv`             | Environment variables for Jenkins Container                              | Not set |
| `master.usePodSecurityContext`    | Enable pod security context (must be `true` if `runAsUser` or `fsGroup` are set) | `true` |
| `master.runAsUser`                | uid that jenkins runs with           | `0`                                       |
| `master.fsGroup`                  | uid that will be used for persistent volume | `0`                                |
| `master.hostAliases`              | Aliases for IPs in `/etc/hosts`      | `[]`                                      |
| `master.serviceAnnotations`       | Service annotations                  | `{}`                                      |
| `master.serviceType`              | k8s service type                     | `ClusterIP`                               |
| `master.servicePort`              | k8s service port                     | `8080`                                    |
| `master.targetPort`               | k8s target port                      | `8080`                                    |
| `master.nodePort`                 | k8s node port                        | Not set                                   |
| `master.healthProbes`             | Enable k8s liveness and readiness probes    | `true`                             |
| `master.healthProbesLivenessTimeout`  | Set the timeout for the liveness probe  | `5`                              |
| `master.healthProbesReadinessTimeout` | Set the timeout for the readiness probe | `5`                               |
| `master.healthProbeLivenessPeriodSeconds` | Set how often (in seconds) to perform the liveness probe | `10`         |
| `master.healthProbeReadinessPeriodSeconds` | Set how often (in seconds) to perform the readiness probe | `10`         |
| `master.healthProbeLivenessFailureThreshold` | Set the failure threshold for the liveness probe | `5`               |
| `master.healthProbeReadinessFailureThreshold` | Set the failure threshold for the readiness probe | `3`               |
| `master.healthProbeLivenessInitialDelay` | Set the initial delay for the liveness probe | `90`               |
| `master.healthProbeReadinessInitialDelay` | Set the initial delay for the readiness probe | `60`               |
| `master.slaveListenerPort`        | Listening port for agents            | `50000`                                   |
| `master.slaveHostPort`            | Host port to listen for agents            | Not set                              |
| `master.slaveKubernetesNamespace` | Namespace in which the Kubernetes agents should be launched  | Not set           |
| `master.disabledAgentProtocols`   | Disabled agent protocols             | `JNLP-connect JNLP2-connect`              |
| `master.csrf.defaultCrumbIssuer.enabled` | Enable the default CSRF Crumb issuer | `true`                             |
| `master.csrf.defaultCrumbIssuer.proxyCompatability` | Enable proxy compatibility | `true`                            |
| `master.cli`                      | Enable CLI over remoting             | `false`                                   |
| `master.slaveListenerServiceType` | Defines how to expose the slaveListener service | `ClusterIP`                    |
| `master.slaveListenerLoadBalancerIP`  | Static IP for the slaveListener LoadBalancer | Not set                       | 
| `master.loadBalancerSourceRanges` | Allowed inbound IP addresses         | `0.0.0.0/0`                               |
| `master.loadBalancerIP`           | Optional fixed external IP           | Not set                                   |
| `master.jmxPort`                  | Open a port, for JMX stats           | Not set                                   |
| `master.extraPorts`               | Open extra ports, for other uses     | `[]`                                      |
| `master.overwriteConfig`          | Replace init scripts and config w/ ConfigMap on boot  | `false`                  |
| `master.ingress.enabled`          | Enables ingress                      | `false`                                   |
| `master.ingress.apiVersion`       | Ingress API version                  | `extensions/v1beta1`                      |
| `master.ingress.hostName`         | Ingress host name                    | Not set                                   |
| `master.ingress.annotations`      | Ingress annotations                  | `{}`                                      |
| `master.ingress.labels`           | Ingress labels                       | `{}`                                      |
| `master.ingress.path`             | Ingress path                         | Not set                                   |
| `master.ingress.tls`              | Ingress TLS configuration            | `[]`                                      |
| `master.backendconfig.enabled`     | Enables backendconfig     | `false`              |
| `master.backendconfig.apiVersion`  | backendconfig API version | `extensions/v1beta1` |
| `master.backendconfig.name`        | backendconfig name        | Not set              |
| `master.backendconfig.annotations` | backendconfig annotations | `{}`                 |
| `master.backendconfig.labels`      | backendconfig labels      | `{}`                 |
| `master.backendconfig.spec`        | backendconfig spec        | `{}`                 |
| `master.route.enabled`            | Enables openshift route              | `false`                                   |
| `master.route.annotations`        | Route annotations                    | `{}`                                      |
| `master.route.labels`             | Route labels                         | `{}`                                      |
| `master.route.path`               | Route path                           | Not set                                   |
| `master.jenkinsUrlProtocol`       | Set protocol for JenkinsLocationConfiguration.xml | Set to `https` if `Master.ingress.tls`, `http` otherwise |
| `master.JCasC.enabled`            | Wheter Jenkins Configuration as Code is enabled or not | `false`                 |
| `master.JCasC.defaultConfig`      | Enables default Jenkins configuration via configuration as code plugin | `false` |
| `master.JCasC.configScripts`      | List of Jenkins Config as Code scripts | `{}`                                    |
| `master.enableXmlConfig`          | enables configuration done via XML files | `true`                               |
| `master.sidecars.configAutoReload` | Jenkins Config as Code auto-reload settings |                                   |
| `master.sidecars.configAutoReload.enabled` | Jenkins Config as Code auto-reload settings (Attention: rbac needs to be enabled otherwise the sidecar can't read the config map) | `false`                                                      |
| `master.sidecars.configAutoReload.image` | Image which triggers the reload | `kiwigrid/k8s-sidecar:0.1.20`           |
| `master.sidecars.other`           | Configures additional sidecar container(s) for Jenkins master | `[]`             |
| `master.initScripts`              | List of Jenkins init scripts         | `[]`                                      |
| `master.credentialsXmlSecret`     | Kubernetes secret that contains a 'credentials.xml' file | Not set               |
| `master.secretsFilesSecret`       | Kubernetes secret that contains 'secrets' files | Not set                        |
| `master.jobs`                     | Jenkins XML job configs              | `{}`                                      |
| `master.overwriteJobs`            | Replace jobs w/ ConfigMap on boot    | `false`                                   |
| `master.installPlugins`           | List of Jenkins plugins to install. If you don't want to install plugins set it to `[]` | `kubernetes:1.18.2 workflow-aggregator:2.6 credentials-binding:1.19 git:3.11.0 workflow-job:2.33` |
| `master.overwritePlugins`         | Overwrite installed plugins on start.| `false`                                   |
| `master.enableRawHtmlMarkupFormatter` | Enable HTML parsing using (see below) | false                                |
| `master.scriptApproval`           | List of groovy functions to approve  | `[]`                                      |
| `master.nodeSelector`             | Node labels for pod assignment       | `{}`                                      |
| `master.affinity`                 | Affinity settings                    | `{}`                                      |
| `master.schedulerName`            | Kubernetes scheduler name            | Not set                                   |
| `master.tolerations`              | Toleration labels for pod assignment | `[]`                                      |
| `master.podAnnotations`           | Annotations for master pod           | `{}`                                      |
| `master.deploymentAnnotations`           | Annotations for master deployment           | `{}`                                      |
| `master.customConfigMap`          | Deprecated: Use a custom ConfigMap   | `false`                                   |
| `master.additionalConfig`         | Deprecated: Add additional config files | `{}`                                   |
| `master.jenkinsUriPrefix`         | Root Uri Jenkins will be served on   | Not set                                   |
| `master.customInitContainers`     | Custom init-container specification in raw-yaml format | Not set                 |
| `master.lifecycle`                | Lifecycle specification for master-container | Not set                           |
| `master.prometheus.enabled`       | Enables prometheus service monitor | `false`                                     |
| `master.prometheus.serviceMonitorAdditionalLabels` | Additional labels to add to the service monitor object | `{}`                       |
| `master.prometheus.serviceMonitorNamespace` | Custom namespace for serviceMonitor | Not set (same ns where is Jenkins being deployed) |
| `master.prometheus.scrapeInterval` | How often prometheus should scrape metrics | `60s`                              |
| `master.prometheus.scrapeEndpoint` | The endpoint prometheus should get metrics from | `/prometheus`                 |
| `master.prometheus.alertingrules` | Array of prometheus alerting rules | `[]`                                        |
| `master.prometheus.alertingRulesAdditionalLabels` | Additional labels to add to the prometheus rule object     | `{}`                                   |
| `master.priorityClassName`        | The name of a `priorityClass` to apply to the master pod | Not set               |
| `master.testEnabled`              | Can be used to disable rendering test resources when using helm template | `true`                         |
| `master.httpsKeyStore.enable`     | Enables https keystore on jenkins master      | `false`      | 
| `master.httpsKeyStore.jenkinsHttpsJksSecretName`     | Name of the secret that already has ssl keystore      | ``      | 
| `master.httpsKeyStore.httpPort`   | Http Port that Jenkins should listen on along with https, it also serves liveness and readiness probs port. When https keystore is enabled servicePort and targetPort will be used as https port  | `8081`   |
| `master.httpsKeyStore.path`       | Path of https keystore file                  |     `/var/jenkins_keystore`     |
| `master.httpsKeyStore.fileName`  | Jenkins keystore filename which will apear under master.httpsKeyStore.path      | `keystore.jks` |
| `master.httpsKeyStore.password`   | Jenkins keystore password                                           | `password` |
| `master.httpsKeyStore.jenkinsKeyStoreBase64Encoded`  | Base64 ecoded Keystore content. Keystore must be converted to base64 then being pasted here  | a self signed cert |
| `networkPolicy.enabled`           | Enable creation of NetworkPolicy resources. | `false`                            |
| `networkPolicy.apiVersion`        | NetworkPolicy ApiVersion             | `networking.k8s.io/v1`                    |
| `rbac.create`                     | Whether RBAC resources are created   | `true`                                    |
| `rbac.readSecrets`                | Whether the Jenkins service account should be able to read Kubernetes secrets    | `false` |
| `serviceAccount.name`             | name of the ServiceAccount to be used by access-controlled resources | autogenerated |
| `serviceAccount.create`           | Configures if a ServiceAccount with this name should be created | `true`         |
| `serviceAccount.annotations`      | Configures annotation for the ServiceAccount | `{}`                              |
| `serviceAccountAgent.name`        | name of the agent ServiceAccount to be used by access-controlled resources | autogenerated |
| `serviceAccountAgent.create`      | Configures if an agent ServiceAccount with this name should be created | `false`         |
| `serviceAccountAgent.annotations` | Configures annotation for the agent ServiceAccount | `{}`                              |


Some third-party systems, e.g. GitHub, use HTML-formatted data in their payload sent to a Jenkins webhooks, e.g. URL of a pull-request being built. To display such data as processed HTML instead of raw text set `master.enableRawHtmlMarkupFormatter` to true. This option requires installation of OWASP Markup Formatter Plugin (antisamy-markup-formatter). The plugin is **not** installed by default, please update `master.installPlugins`.

### Jenkins Agent

| Parameter                  | Description                                     | Default                |
| -------------------------- | ----------------------------------------------- | ---------------------- |
| `agent.alwaysPullImage`    | Always pull agent container image before build  | `false`                |
| `agent.customJenkinsLabels`| Append Jenkins labels to the agent              | `{}`                   |
| `agent.enabled`            | Enable Kubernetes plugin jnlp-agent podTemplate | `true`                 |
| `agent.image`              | Agent image name                                | `jenkins/jnlp-slave`   |
| `agent.imagePullSecret`    | Agent image pull secret                         | Not set                |
| `agent.tag`                | Agent image tag                                 | `3.27-1`               |
| `agent.privileged`         | Agent privileged container                      | `false`                |
| `agent.resources`          | Resources allocation (Requests and Limits)      | `{requests: {cpu: 512m, memory: 512Mi}, limits: {cpu: 512m, memory: 512Mi}}`|
| `agent.volumes`            | Additional volumes                              | `[]`                   |
| `agent.envVars`            | Environment variables for the agent Pod         | `[]`                   |
| `agent.command`            | Executed command when side container starts     | Not set                |
| `agent.args`               | Arguments passed to executed command            | Not set                |
| `agent.sideContainerName`  | Side container name in agent                    | jnlp                   |
| `agent.TTYEnabled`         | Allocate pseudo tty to the side container       | false                  |
| `agent.containerCap`       | Maximum number of agent                         | 10                     |
| `agent.podName`            | Agent Pod base name                             | Not set                |
| `agent.idleMinutes`        | Allows the Pod to remain active for reuse       | 0                      |
| `agent.yamlTemplate`       | The raw yaml of a Pod API Object to merge into the agent spec | Not set                |
| `agent.slaveConnectTimeout`| Timeout in seconds for an agent to be online    | 100                    |


## Configuration as Code
Jenkins Configuration as Code is now a standard component in the Jenkins project.  Add a key under configScripts for each configuration area, where each corresponds to a plugin or section of the UI.  The keys (prior to | character) are just labels, and can be any value.  They are only used to give the section a meaningful name.  The only restriction is they must conform to RFC 1123 definition of a DNS label, so may only contain lowercase letters, numbers, and hyphens.  Each key will become the name of a configuration yaml file on the master in /var/jenkins_home/casc_configs (by default) and will be processed by the Configuration as Code Plugin during Jenkins startup.  The lines after each | become the content of the configuration yaml file.  The first line after this is a JCasC root element, eg jenkins, credentials, etc.  Best reference is the Documentation link here: https://<jenkins_url>/configuration-as-code.  The example below creates ldap settings:

```yaml
configScripts:
  ldap-settings: |
    jenkins:
      securityRealm:
        ldap:
          configurations:
            configurations:
              - server: ldap.acme.com
                rootDN: dc=acme,dc=uk
                managerPasswordSecret: ${LDAP_PASSWORD}
              - groupMembershipStrategy:
                  fromUserRecord:
                    attributeName: "memberOf"
```

Further JCasC examples can be found [here.](https://github.com/jenkinsci/configuration-as-code-plugin/tree/master/demos)

### Config as Code with and without auto-reload
Config as Code changes (to master.JCasC.configScripts) can either force a new pod to be created and only be applied at next startup, or can be auto-reloaded on-the-fly.  If you choose `master.sidecars.autoConfigReload.enabled: true`, a second, auxiliary container will be installed into the Jenkins master pod, known as a "sidecar".  This watches for changes to configScripts, copies the content onto the Jenkins file-system and issues a CLI command via SSH to reload configuration.  The admin user (or account you specify in master.adminUser) will have a random SSH private key (RSA 4096) assigned unless you specify a key in `master.adminSshKey`.  This will be saved to a k8s secret.  You can monitor this sidecar's logs using command `kubectl logs <master_pod> -c jenkins-sc-config -f`
If you want to enable auto-reload then you also need to configure rbac as the container which triggers the reload needs to watch the config maps.

```yaml
master:
  JCasC:
    enabled: true
  sidecars:
    configAutoReload:
      enabled: true
rbac:
  install: true
```

### Auto-reload with non-Jenkins identities
When enabling LDAP or another non-Jenkins identity source, the built-in admin account will no longer exist.  Since the admin account is used by the sidecar to reload config, in order to use auto-reload, you must change the .master.adminUser to a valid username on your LDAP (or other) server.  If you use the matrix-auth plugin, this user must also be granted Overall\Administer rights in Jenkins.  Failure to do this will cause the sidecar container to fail to authenticate via SSH and enter a restart loop.  You can enable LDAP using the example above and add a Config as Code block for matrix security that includes:
```yaml
configScripts:
  matrix-auth: |
    jenkins:
      authorizationStrategy:
        projectMatrix:
          grantedPermissions:
          - "Overall/Administer:<AdminUser_LDAP_username>"
```
You can instead grant this permission via the UI. When this is done, you can set `master.sidecars.configAutoReload.enabled: true` and upon the next Helm upgrade, auto-reload will be successfully enabled.


## Backup

Adds a backup CronJob for jenkins, along with required RBAC resources.

### Backup Values

| Parameter                                | Description                                                       | Default                           |
| ---------------------------------------- | ----------------------------------------------------------------- | --------------------------------- |
| `backup.enabled`                         | Enable the use of a backup CronJob                                | `false`                           |
| `backup.schedule`                        | Schedule to run jobs                                              | `0 2 * * *`                       |
| `backup.labels`                          | Backup pod labels                                                 | `{}`                              |
| `backup.annotations`                     | Backup pod annotations                                            | `{}`                              |
| `backup.image.repo`                      | Backup image repository                                           | `maorfr/kube-tasks`               |
| `backup.image.tag`                       | Backup image tag                                                  | `0.2.0`                           |
| `backup.extraArgs`                       | Additional arguments for kube-tasks                               | `[]`                              |
| `backup.existingSecret`                  | Environment variables to add to the cronjob container             | `{}`                              |
| `backup.existingSecret.*`                | Specify the secret name containing the AWS or GCP credentials     | `jenkinsaws`                      |
| `backup.existingSecret.*.awsaccesskey`   | `secretKeyRef.key` used for `AWS_ACCESS_KEY_ID`                   | `jenkins_aws_access_key`          |
| `backup.existingSecret.*.awssecretkey`   | `secretKeyRef.key` used for `AWS_SECRET_ACCESS_KEY`               | `jenkins_aws_secret_key`          |
| `backup.existingSecret.*.gcpcredentials` | Mounts secret as volume and sets `GOOGLE_APPLICATION_CREDENTIALS` | `credentials.json`                |
| `backup.env`                             | Backup environment variables                                      | `[]`                              |
| `backup.resources`                       | Backup CPU/Memory resource requests/limits                        | Memory: `1Gi`, CPU: `1`           |
| `backup.destination`                     | Destination to store backup artifacts                             | `s3://jenkins-data/backup`        |

### Restore from backup

To restore a backup, you can use the `kube-tasks` underlying tool called [skbn](https://github.com/maorfr/skbn), which copies files from cloud storage to Kubernetes.
The best way to do it would be using a `Job` to copy files from the desired backup tag to the Jenkins pod.
See the [skbn in-cluster example](https://github.com/maorfr/skbn/tree/master/examples/in-cluster) for more details.
