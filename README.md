This repository contains the <b>Edge ToolKit</b>, or <b>ETK</b> for
short. The <b>ETK</b> is a collection of binaries and golang libraries
for managing requests that that flow into, out of, or within
Kubernetes-based cloud native applications.

## Components of the ETK

Commands:

- [edgectl](docs/edgectl.md) - CLI for controlling the traffic into, out of, or within a Kubernetes cluster
- [teleproxy](docs/teleproxy.md) - connect locally running code to a remote Kubernetes cluster
- [watt](docs/watt.md) - trigger actions when Kubernetes and/or consul resources are updated
- [kubeapply](docs/kubeapply.md) - apply Kubernetes manifests with templating, docker builds, and feedback
- [k3sctl](docs/k3sctl.md) - run/manage a lightweight local Kubernetes cluster and docker registry for testing
- [kubestatus](docs/kubestatus.md) - display and update the status of arbitrary Kubernetes resources

Libraries:

- [supervisor](https://godoc.org/github.com/datawire/teleproxy/pkg/supervisor) - a library (or very lightweight framework) for helping with error handling, startup/shutdown order, and guaranteed resource cleanup amongst groups of related goroutines
- [k8s](https://godoc.org/github.com/datawire/teleproxy/pkg/k8s) - an easy-to-use facade around the client-go library
- [dtest](https://godoc.org/github.com/datawire/teleproxy/pkg/dtest) - testing related utilities

## Changelog

See [CHANGELOG](./CHANGELOG.md) for notes on the current release.

## Linux downloads

```bash
curl https://datawire-static-files.s3.amazonaws.com/edgectl/0.7.0/linux/amd64/edgectl -o edgectl && chmod a+x edgectl
curl https://datawire-static-files.s3.amazonaws.com/teleproxy/0.7.0/linux/amd64/teleproxy -o teleproxy && chmod a+x teleproxy
curl https://datawire-static-files.s3.amazonaws.com/watt/0.7.0/linux/amd64/watt -o watt && chmod a+x watt
curl https://datawire-static-files.s3.amazonaws.com/kubeapply/0.7.0/linux/amd64/kubeapply -o kubeapply && chmod a+x kubeapply
curl https://datawire-static-files.s3.amazonaws.com/k3sctl/0.7.0/linux/amd64/k3sctl -o k3sctl && chmod a+x k3sctl
curl https://datawire-static-files.s3.amazonaws.com/kubestatus/0.7.0/linux/amd64/kubestatus -o kubestatus && chmod a+x kubestatus
```

## Darwin downloads

```bash
curl https://datawire-static-files.s3.amazonaws.com/edgectl/0.7.0/darwin/amd64/edgectl -o edgectl && chmod a+x edgectl
curl https://datawire-static-files.s3.amazonaws.com/teleproxy/0.7.0/darwin/amd64/teleproxy -o teleproxy && chmod a+x teleproxy
curl https://datawire-static-files.s3.amazonaws.com/watt/0.7.0/darwin/amd64/watt -o watt && chmod a+x watt
curl https://datawire-static-files.s3.amazonaws.com/kubeapply/0.7.0/darwin/amd64/kubeapply -o kubeapply && chmod a+x kubeapply
curl https://datawire-static-files.s3.amazonaws.com/k3sctl/0.7.0/darwin/amd64/k3sctl -o k3sctl && chmod a+x k3sctl
curl https://datawire-static-files.s3.amazonaws.com/kubestatus/0.7.0/darwin/amd64/kubestatus -o kubestatus && chmod a+x kubestatus
```

## Repository Organization

This repository follows the standard golang package layout.

#### Commands

You will
find the source code for the binary commands at
`<repo>/cmd/<name>/...`. The main function for each command should be
in a file named `main.go` within the command directory,
e.g. `<repo>/cmd/<name>/main.go`.

#### Packages

Public packages can be found at `<repo>/pkg/<name>/...`, and internal
packages can be found at `<repo>/internal/pkg/<name>/...`.


## Building

Use `make check` and `make build` for running formal test and
builds. You can use `make help` to find other useful targets.

## Developing

The fastest way to install any command from source, e.g. for inner
loop development is to run `go install ./cmd/<name>/...`.

The fastest way to run tests is to run `go test ./<path>/...`

### Testing dependencies

The tests require a Kubernetes cluster and a docker registry to
run. By default these are spun up locally on demand. You can also
supply your own cluster and/or registry.

#### On demand cluster and registry

These are left running in between test runs for performance. If you
want a fully clean test run, you can use `docker ps` and `docker kill`
to clean these up. You can also run `go install ./cmd/k3sctl/...` to
get the `k3sctl` command and use `k3sctl up` and `k3sctl down` to
control these containers. Use the `k3sctl help` command to find other
useful commands, e.g. you can use `k3sctl config -o /tmp/k3s.yaml` to
get a kubeconfig for kusing kubectl against the testing cluster.

#### Supplying your own cluster and registry

Use the `DTEST_REGISTRY` environment variable to make the tests use a
given docker registry for testing. You will need to be authenticated
and have permission to push to whatever registry you supply.

Use the `DTEST_KUBECONFIG` environment variable to make the tests use
a particular Kubernetes cluster for testing. Note that the tests may
be destructive, so make sure you use a suitable cluster. This cluster
will need to be able to pull from whatever registry you supply.
