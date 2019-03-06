# Advanced config

The operator has two ways for configuration:

* Per namespace backups. Optimal for shared clusters
* Global settings with namespaced schedules. Optimal for private clusters

## Environment variables

* `BACKUP_IMAGE` URL for the restic image, default: `172.30.1.1:5000/myproject/restic`
* `BACKUP_ANNOTATION` the annotation to be used for filtering, default: `appuio.ch/backup`
* `BACKUP_CHECKSCHEDULE` the default check schedule, default: `0 0 * * 0`
* `BACKUP_PODFILTER` the filter used to find the backup pods, default: `backupPod=true`
* `BACKUP_DATAPATH` where the PVCs should get mounted in the container, default `/data`
* `BACKUP_JOBNAME` names for the backup job objects in OpenShift, default: `backupjob`
* `BACKUP_PODNAME` names for the backup pod objects in OpenShift, default: `backupjob-pod`
* `BACKUP_RESTARTPOLICY` set the RestartPolicy for the backup jobs. According to the [docs](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/) this should be `OnFailure` for jobs that terminate, default: `OnFailure`
* `BACKUP_METRICBIND` set the bind address for the prometheus endpoint, default: `:8080`
* `BACKUP_PROMURL` set the operator wide default prometheus push gateway, default `http://127.0.0.1/`
* `BACKUP_BACKUPCOMMANDANNOTATION` set the annotation name where the backup commands are stored, default `appuio.ch/backupcommand`
* `BACKUP_PODEXECROLENAME` set the rolename that should be used for pod command execution, default `pod-executor`
* `BACKUP_PODEXECACCOUNTNAME` set the service account name that should be used for the pod command execution, default: `pod-executor`
* `BACKUP_GLOBALACCESSKEYID` set the S3 access key id to be used globaly
* `BACKUP_GLOBALSECRETACCESSKEY` set the S3 secret access key to be used globaly
* `BACKUP_GLOBALREPOPASSWORD` set the restic repository password to be used globaly
* `BACKUP_GLOBALKEEPJOBS` set the count of jobs to keep globally
* `BACKUP_GLOBALS3ENDPOINT` set the S3 endpoint to be used globally
* `BACKUP_GLOBALS3BUCKET` set the S3 bucket to be used globally
* `BACKUP_GLOBALSTATSURL` set the URL for wrestic to post additional metrics gloablly, default `""`
* `BACKUP_GLOBALRESTORES3BUCKET` set the global restore S3 bucket for restores
* `BACKUP_GLOBALRESTORES3ENDPOINT` set the global restore S3 endpoint for the restores (needs the scheme [http/https]
* `BACKUP_GLOBALRESTORES3ACCESKEYID` set the global resotre S3 accessKeyID for restores
* `BACKUP_GLOBALRESTORES3SECRETACCESSKEY` set the global restore S3 SecretAccessKey for restores

You only need to adjust `BACKUP_IMAGE` everything else can be left default.

## Global settings

Each variable starting with `BACKUP_GLOBAL*` is used to configure a global default for all namespaces. F.e. if you configure the S3 bucket and credentials you won't have to specify them in the schedule or backup CRDs.

!!! Note

    It is possible to overwrite the global settings. Simply set the specific configuration in the CRD and it will use that instead.

## Manual Installation
All required definitions for the installation are located at `manifest/install/`:

```bash
kubectl apply -f manifest/install/
```

Please be aware that these manifests are intended for dev and as examples. They are not the official way to install the operator in production. For this we provide a helm chart at https://github.com/appuio/charts.
You may need to adjust the namespaces in the manifests. There are various other examples under `manifest/examples/`.