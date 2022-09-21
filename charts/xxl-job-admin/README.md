## Usage

[Helm](https://helm.sh) must be installed to use the charts.  Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

  helm repo add xxl-job https://joelee2012.github.io/helm-charts

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages.  You can then run `helm search repo
xxl-job` to see the charts.

To install the xxl-job-admin chart:

    helm install my-xxl-job-admin xxl-job/xxl-job-admin

To uninstall the chart:

    helm uninstall my-xxl-job-admin