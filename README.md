# honeybee

## Fire up cluster

```bash
cp .env.placeholder .env
vim .env

docker swarm init --advertise-addr $(curl -s https://ifconfig.co)
docker network create -d overlay --attachable honeybee
docker stack deploy -c cluster.yml honeybee
```

```bash
# in master node
docker exec -it honeybee_mongo.1.<task_id> mongo -u honeybee
```

```js
use honeybee

db.createUser({
  user: "worker",
  pwd: passwordPrompt(), // or cleartext password
  roles: [{ role: "readWrite", db: "honeybee" }],
});
```

```js
authenticationRestrictions: [
  {
    clientSource: ["<ip|cidr>"],
  },
];
```

## Show cluster health

```bash
make stats
```

## Deploy additional worker nodes

```bash
cd tf
terraform init
terraform apply
```

### Teardown workers

```bash
cd tf
terraform destroy
```

## Running migration task

```bash
NODE_OPTIONS=--max-old-space-size=32768 honeybee --help
```
