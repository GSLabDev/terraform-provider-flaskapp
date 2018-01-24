# Terraform HTTP File Upload Provider

This is the repository for the Terraform HTTP File Upload Provider, which one can use
with Terraform to work upload any file on WebServer over HTTP.

For general information about Terraform, visit the [official website][3] and the
[GitHub project page][4].

[3]: https://terraform.io/
[4]: https://github.com/hashicorp/terraform

# Using the Provider

The current version of this provider requires Terraform v0.10.2 or higher to
run.

Note that you need to run `terraform init` to fetch the provider before
deploying. Read about the provider split and other changes to TF v0.10.0 in the
official release announcement found [here][4].

[4]: https://www.hashicorp.com/blog/hashicorp-terraform-0-10/

## Full Provider Documentation

The provider is usefull for uploading any file to webserver over HTTP using Terraform.

### Example

```hcl
# Upload a file
resource "httpfileupload_file" "my_file"{
  host_url = "localhost:8080/upload"
  file_path = "/home/user/Documents/hello.txt"
}
```

# Building The Provider

**NOTE:** Unless you are [developing][7] or require a pre-release bugfix or feature,
you will want to use the officially released version of the provider (see [the
section above][8]).

[7]: #developing-the-provider
[8]: #using-the-provider


## Cloning the Project

First, you will want to clone the repository to
`$GOPATH/src/github.com/terraform-providers/terraform-provider-httpfileupload`:

```sh
mkdir -p $GOPATH/src/github.com/terraform-providers
cd $GOPATH/src/github.com/terraform-providers
git clone git@github.com:terraform-providers/terraform-provider-httpfileupload
```

## Running the Build

After the clone has been completed, you can enter the provider directory and
build the provider.

```sh
cd $GOPATH/src/github.com/terraform-providers/terraform-provider-httpfileupload
make build
```

## Installing the Local Plugin

After the build is complete, copy the `terraform-provider-httpfileupload` binary into
the same path as your `terraform` binary, and re-run `terraform init`.

After this, your project-local `.terraform/plugins/ARCH/lock.json` (where `ARCH`
matches the architecture of your machine) file should contain a SHA256 sum that
matches the local plugin. Run `shasum -a 256` on the binary to verify the values
match.

# Developing the Provider

If you wish to work on the provider, you'll first need [Go][9] installed on your
machine (version 1.9+ is **required**). You'll also need to correctly setup a
[GOPATH][10], as well as adding `$GOPATH/bin` to your `$PATH`.

[9]: https://golang.org/
[10]: http://golang.org/doc/code.html#GOPATH

See [Building the Provider][11] for details on building the provider.

[11]: #building-the-provider

# Testing the Provider

**PreRequisite:** WebServer should be running with specific port allowed to accept file upload.

## Configuring Environment Variables

Most of the tests in this provider require a comprehensive list of environment
variables to run. See the individual `*_test.go` files in the
[`httpfileupload/`](httpfileupload/) directory for more details. The next section also
describes how you can manage a configuration file of the test environment
variables.

### Using the `.tf-httpfileupload-devrc.mk` file

The [`tf-httpfileupload-devrc.mk.example`](tf-httpfileupload-devrc.mk.example) file contains
an up-to-date list of environment variables required to run the acceptance
tests. Copy this to `$HOME/.tf-httpfileupload-devrc.mk` and change the permissions to
something more secure (ie: `chmod 600 $HOME/.tf-httpfileupload-devrc.mk`), and
configure the variables accordingly.

## Running the Acceptance Tests

After this is done, you can run the acceptance tests by running:

```sh
$ make testacc
```

If you want to run against a specific set of tests, run `make testacc` with the
`TESTARGS` parameter containing the run mask as per below:

```sh
make testacc TESTARGS="-run=TestAccHttpFileUpload"
```

This following example would run all of the acceptance tests matching
`TestAccHttpFileUpload`. Change this for the specific tests you want to
run.

## Example Usage

```hcl
# Upload a file
resource "httpfileupload_file" "my_file"{
  host_url = "localhost:8080/upload"
  file_path = "/home/user/Documents/hello.txt"
}
```

## Argument Reference

The following arguments are supported:

* `host_url` - (Required) The Url for http file upload
* `file_path` - (Required) The file path of the File to be uploaded

The main.tf file contains the microservices of how to call the providers and resources. We need to specify required details for resource creation in this file.