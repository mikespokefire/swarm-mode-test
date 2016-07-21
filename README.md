# Vagrant demo of Dockers Swarm Mode

## Running locally

To get your swarm mode cluster setup locally, bring up the machines with vagrant:

```
vagrant up
```

Once they have provisioned, you can ssh into the swarm-manager and check on the status of your cluster:

```
vagrant ssh swarm-manager
docker node ls
```

You should see something like:

```
vagrant@swarm-manager:~$ docker node ls
ID                           HOSTNAME                   MEMBERSHIP  STATUS  AVAILABILITY  MANAGER STATUS
0o2lwuuvn219msici53vdc7po    swarm-node1.example.com    Accepted    Ready   Active        
2c4oewsf2bk1d704nyyp2o6lw    swarm-node3.example.com    Accepted    Ready   Active        
7puefkwx0itq7bi9f7dw1wbw6 *  swarm-manager.example.com  Accepted    Ready   Active        Leader
bmgm8ftvn7wzsxinyps8kn688    swarm-node2.example.com    Accepted    Ready   Active        
```

Your swarm mode cluster is now up and running.
