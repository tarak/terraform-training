# Erstellen einer virtuellen Maschine in AWS mit dem AWS CLI

## SSH-Schlüssel erzeugen

### Windows

<img src="https://www.ssh.com/s/pyttygen-created-ssh-key-479x471-reHlFacl.png" />

Public Key speichern unter `%USERPROFILE%\.ssh\id_rsa.aws.pub`.

### MacOS & Linux

```
ssh-keygen -t rsa -b 2048  -C "YOUR_NAME" -f ~/.ssh/id_rsa.aws
chmod 0400 ~/.ssh/id_rsa.aws
```

## Upload Keypair

```
aws ec2 import-key-pair --key-name "YOUR_NAME" \
    --public-key-material file://~/.ssh/id_rsa.aws.pub \
    --profile terraform-training
```

## Security Gruppe

```
aws ec2 create-security-group --group-name ssh-external \
    --description "Zugriff über SSH von extern" \
    --profile terraform-training

aws ec2 authorize-security-group-ingress --group-name ssh-external \
    --protocol tcp --port 22 --cidr 0.0.0.0/0 \
    --profile terraform-training
```

## Erstellen von VM's

```
aws ec2 run-instances --image-id ami-05af84768964d3dc0 \
    --instance-type t2.nano --key-name "YOUR_NAME" \
    --security-groups ssh-external --count 2 \
    --profile terraform-training
```
