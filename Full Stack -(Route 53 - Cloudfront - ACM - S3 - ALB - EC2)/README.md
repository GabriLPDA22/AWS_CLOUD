# Desafío: Full Stack - AWS

## Arquitectura

La siguiente arquitectura muestra el flujo de una aplicación desplegada en AWS, utilizando varios servicios como Route 53, CloudFront, ACM, S3, ALB y EC2.

![Arquitectura AWS](./imagen/Cloudfront.png)


## Componentes

### S3

El servicio de S3 se utiliza para alojar contenido estático como un sitio web. Un ejemplo de política de S3 podría ser:

```json
{
  "Version": "2008-10-17",
  "Id": "Policy",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket/*"
    }
  ]
}
```

### CloudFront

CloudFront se configura para distribuir el contenido del sitio web. La política de CloudFront que permite el acceso al servicio podría parecerse a:

```json
{
  "Version": "2008-10-17",
  "Id": "PolicyForCloudFrontPrivateContent",
  "Statement": [
    {
      "Sid": "AllowCloudFrontServicePrincipal",
      "Effect": "Allow",
      "Principal": {
        "Service": "cloudfront.amazonaws.com"
      },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::xxxxxxxxxxxx/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::xxxxxxxxxxxx:distribution/xxxxxxxxxxxx"
        }
      }
    }
  ]
}
```

### EC2

Para el servicio EC2, se crea un Grupo de Seguridad que permite el tráfico en el puerto 80. Un script de inicio podría ser:

```bash
#!/bin/bash
dnf update -y
dnf install -y docker
service docker start
systemctl enable docker.service
docker pull santosderek/spjuiceshop
docker run -d -p 80:3000 santosderek/spjuiceshop
```

## Video Tutorial

Para más información y un tutorial paso a paso, visita los siguientes enlaces:

- [Instalar un sitio web estático con S3]([https://youtu.be/xxNahu0btIk](https://youtu.be/4bxVDFwqd5o))
- [Configurar EC2 y ALB](https://youtu.be/4bxDFvqd5o)
