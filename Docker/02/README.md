# Docker Swarm Project

## Cluster INIT
we need 2 linux machines, 1 for control and 1 for node1 with docker installed.

1. we execute this command on the comtrol machine:

    ```bash
    docker swarm init 
    ```

    it gives a join command for the worker node to join the swarm cluster:

    ```bash
    docker swarm join --token mytoken control:2377
    ```

2. check if the node(s) have successfully joined the cluster.

   ```txt
   docker node ls
   ```

## Deploy the stack
3. deploy the tomcat stack on the cluster.

   ```bash
   docker stack deploy -c stack.yml tomcat-stack
   ```

4. to check the status of services and containers of tomcat-stack in the cluster :

   ```bash
   docker stack services tomcat-stack
   ```

   ```bash
   docker stack ps tomcat-stack
   ```

