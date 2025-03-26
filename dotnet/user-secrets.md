### Init user secret for a project

```shell
dotnet user-secrets init
dotnet user-secrets init --project projectname
```

> It will generate a UserSecretsId in c project file. you can copy same id to other projects if you want to use same secrete file.

### Set a secret with following command

```shell
dotnet user-secrets set "key" "value"
```

> you can set nested json keys with : eg. connectionString:postgres

### Set a multiple secret from a json file

- create a json file with name "secrets.json" with all the secrete in below format.

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

- you can keep this file in project root and put in gitignore if you want to nodejs type convention.
- then you can following command to set all secretes together.

```shell
cat ./input.json | dotnet user-secrets set
```

### list all scretes

```shell
dotnet user-secrets list
```

### remove secret

```shell
dotnet user-secrets remove keyname
```

### clear all scretes

```shell
dotnet user-secrets clear
```
