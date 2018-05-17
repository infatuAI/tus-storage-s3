# Tus.Storage.S3

A	plugin for the Tus (https://github.com/jpscaletti/tus) package.
Provides a storage backend based on AWS S3 or compatible servers.

## Installation

The package can be installed by adding `tus_storage_s3` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [
    {:tus, "~> 0.1.0"},
    {:tus_storage_s3, "~> 0.1.0"},
  ]
end
```

## Configuration

- `storage`:
	Set it as `Tus.Storage.S3`.

- `s3_host`:
  Optional — Amazon S3 host (https://s3.amazonaws.com) will be used by default.

- `s3_bucket`:
  Name of the bucket were the uploaded files'll be stored

- `s3_base_path`:
  Optipnal – This allows you to store the files in a "subfolder" of the bucket.

- `base_url`:
  If not defined, this'll be `s3_host/s3_bucket`

In order to function properly, the user accessing the bucket must have at least the
following AWS IAM policy permissions for the bucket and all of its subresources:

```
s3:AbortMultipartUpload
s3:DeleteObject
s3:GetObject
s3:ListMultipartUploadParts
s3:PutObject
```

Tus.Storage.S3 uses the ExAWS package, so you'll need to add valid AWS keys to its config.

```elixir
config :ex_aws,
  access_key_id: [{:system, "AWS_ACCESS_KEY_ID"}, :instance_role],
  secret_access_key: [{:system, "AWS_SECRET_ACCESS_KEY"}, :instance_role]
```

This means it will try to resolve credentials in this order

- a. Look for the AWS standard AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY environment variables
- b. Resolve credentials with IAM

Consult the (ExAWS documentation)[https://hexdocs.pm/ex_aws/ExAws.html#module-aws-key-configuration] for more details
