# Tus.Storage.S3

S3 (or compatible) storage backend for the [Tus server](https://hex.pm/packages/tus)

## Installation

The package can be installed by adding `tus_cache_redis` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [
    {:tus, "~> 0.1.1"},
    {:tus_storage_s3, "~> 0.1.0"},
  ]
end
```

## Configuration

- `storage`: Set it as `Tus.Storage.S3`.
- `s3_bucket`: The name of your bucket
- `s3_host`: Optional. "https://s3.amazonaws.com" by default
- `s3_prefix`: Optional. Prefix added to all files. Empty by default

In order to allow this backend to function properly, the user accessing the bucket must have at least the
following AWS IAM policy permissions for the bucket and all of its subresources:

```
s3:AbortMultipartUpload
s3:DeleteObject
s3:GetObject
s3:ListMultipartUploadParts
s3:PutObject
```

Furthermore, this uses the ExAWS package, so you'll need to add valid AWS keys to its config.

```elixir
config :ex_aws,
  access_key_id: [{:system, "AWS_ACCESS_KEY_ID"}, :instance_role],
  secret_access_key: [{:system, "AWS_SECRET_ACCESS_KEY"}, :instance_role]
```

This means it will try to resolve credentials in this order

- a. Look for the AWS standard AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY environment variables
- b. Resolve credentials with IAM

Consult the (ExAWS documentation)[https://hexdocs.pm/ex_aws/ExAws.html#module-aws-key-configuration] for more details
