<p align="center">
  <a href="https://docs.tflibsonnet.com/">
    <picture>
      <img
        alt="tf.libsonnet"
        width="400px"
        src="https://github.com/tf-libsonnet/assets/raw/main/imgs/logo-with-slogan-color-long.png?raw=true"
      >
    <picture>
  </a>
</p>

```jsonnet
local aws = import 'github.com/tf-libsonnet/hashicorp-aws/main.libsonnet';
local tf = import 'github.com/tf-libsonnet/core/main.libsonnet';

local o =
  aws.provider.new(region='us-east-1', src='hashicorp/aws')
  + aws.data.ami.new(
    'ubuntu',
    most_recent=true,
    owners=['099720109477'],
    filter=[
      aws.data.ami.filter.new(
        name='name',
        values=['ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*'],
      ),
      aws.data.ami.filter.new(
        name='virtualization-type',
        values=['hvm'],
      ),
    ],
  )
  + aws.instance.new(
    'web',
    ami=o._ref.data.aws_ami.ubuntu.get('id'),
    instance_type='t3.micro',
  )
  + tf.withOutput('arn', o._ref.aws_instance.web.get('arn'));

o
```

Learn more at [docs.tflibsonnet.com](https://docs.tflibsonnet.com/docs/).
