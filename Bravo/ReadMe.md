Let's try to build from bottom to top.

1. 2 dir to be created in node01
2. 2 persistantvolume to be created -> pv.yaml
3. 2 persistantvolumeclaim to be created -> pv.yaml
4. 1 secret to be created. -> sec.yaml just for reference about how we can create it.
   Use imperative command: `kubectl create secret generic drupal-mysql-secret --from-literal=MYSQL_ROOT_PASSWORD=xxx --from-literal=MYSQL_DATABASE=xxx --from-literal=MYSQL_USER=xxx`
5. mysql container along with secret import & persistantvolume -> mysql.yaml
   note: 
   need to import secerts individually to clear this task like.
 
         env:
         - name: MYSQL_DATABASE
           valueFrom:
             secretKeyRef:
               name: drupal-mysql-secret
               key: MYSQL_DATABASE
              
   But I like to use below but its supported from v1.6
       
         envFrom:
         - secretRef:
             name: drupal-mysql-secret
 
6. mysql service to expose as clusterip.
   Use imperative command: `kubectl expose deployment drupal-mysql --port=3306 --target-port=3306 --name=drupal-mysql-service`
   
7. drupal init container + container along with persistantvolume -> drupal.yaml
8. drupal service to expose as nodeport. -> np.yaml
 
   