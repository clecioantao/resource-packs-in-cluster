# Example: rabbitmq resource using a Kubernetes StatefulSet

This example configures a [RabbitMQ - type AMQP](https://developer.humanitec.com/platform-orchestrator/reference/resource-types/#amqp) Resource Definition using Kubernetes `StatefulSet`.

<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| terraform | >= 1.3.0 |
| humanitec | ~> 1.0 |

## Providers

| Name | Version |
|------|---------|
| humanitec | ~> 1.0 |

## Modules

| Name | Source | Version |
|------|--------|---------|
| rabbitmq\_basic | ../../humanitec-resource-defs/rabbitmq/basic | n/a |

## Resources

| Name | Type |
|------|------|
| [humanitec_resource_definition_criteria.rabbitmq_basic](https://registry.terraform.io/providers/humanitec/humanitec/latest/docs/resources/resource_definition_criteria) | resource |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| prefix | Prefix of the created resources | `string` | `"hum-rp-rabbitmq-ex-"` | no |
<!-- END_TF_DOCS -->

## Deploy and use this example

To deploy this resource definition, run these commands below:
```bash
git clone https://github.com/humanitec-architecture/resource-packs-in-cluster

cd resource-packs-in-cluster/examples/rabbitmq/

humctl login
humctl config set org YOUR-ORG

terraform init
terraform plan
terraform apply
```

The created Resource Definition can be used in your Score file like illustrated below:
```yaml
apiVersion: score.dev/v1b1
metadata:
  name: my-workload
containers:
  my-container:
    image: nginx:latest # this container image is just used as an example, it's not talking to rabbitmq.
    variables:
      RABBIT_USER: ${resources.my-rabbit.username}
      RABBIT_PASSWORD: ${resources.my-rabbit.password}
      RABBIT_VHOST: ${resources.my-rabbit.vhost}
resources:
  my-rabbit:
    type: amqp
```

This Score file when deployed to Humanitec will provision the `rabbitmq` message broker and inject the outputs in the associated environment variable.

Here is how to deploy this Score file, for example to the `rabbit-example` Application and `development` Environment:
```bash
humctl create app rabbit-example

humctl score deploy \
    -f score.yaml \
    --app rabbit-example \
    --env development
```