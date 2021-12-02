#In main.yaml there are 2 files mentioned one is stack and another is client.yaml. Stack is to create kubernetes cluster and deploy jenkins & prometheus grafana on it. CLient.yaml file is to create vpn server. Read this file to end to get more details.

#Pre-requisites

* OS used is ubuntu 20.04
* Minimum requirement is 2GB RAM and 2CPU
* Ansible is installed. 
* Working SSH connection.
* Root SSH user.
* uncomment remote_user = root line  in /etc/ansible/ansible.cfg (Probably on line 107)
* Either make entry in /etc/ansible/hosts file according to host file which is their in this directory or just change the host ip in the file.

#command to run ansible-playbook with the hosts file in current directory
* ansible-playbook -i hosts single_node.yaml

#command to run ansible playbook with default /etc/ansible/hosts file
* ansible-playbook  single_node.yaml

#Once the deployment is done than run below command to create ingress and haproxy

* ansible-playbook haproxy.yaml 

==========================================================================
# jenkins and grafana.
========================

* To access grafana
  kubectl port-forward deployment/prometheus-grafana 3000

#Passwd for grafana is 
username: admin
password: prom-operator
=========================


#passwd for jenkins
=====================
* To access jenkins you can use port forwading as well
  kubectl port-forward deployment_name 8080

kubectl get pods -n jenkins
kubectl logs pod_name -n jenkins

and you will get the passwd

# if you find it difficult than you can use the below mentioned way to get passwd
kubectl exec -it jenkins-559d8cd85c-cfcgk cat  /var/jenkins_home/secrets/initialAdminPassword -n jenkins

#To install opwnvpn and wireguard make entry in hosts file. Under client IP, PUt the server IP on which you want to create the openvpn and wireguard server.

