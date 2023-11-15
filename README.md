# deploy this daemonset to kubernetes

kubectl -n your_namespace apply -f wazuh-daemonset3.yml
kubectl -n your_namespace get all

ex : https://wazuh_dashboard_ip:8080/

check your wazuh dashboard - agents, the 5 pods of daemonset wazuh agents should listed there, and its status are active


# bonus, upgrade wazuh agent version via wazuh manager api

create new user with role agents_admin in your wazuh dashboard - user 

for steps below :

change <user>:<password> with your new user password that you've just created above

change localhost with your_wazuh_manager_ip

TOKEN=$(curl -u <user>:<password> -k -X GET "https://localhost:55000/security/user/authenticate?raw=true")

curl -k -X PUT "https://localhost:55000/agents/upgrade?agents_list=002,003&pretty=true" -H  "Authorization: Bearer $TOKEN"

curl -k -X GET "https://localhost:55000/agents/upgrade_result?agents_list=002,003&pretty=true" -H  "Authorization: Bearer $TOKEN"

curl -k -X GET "https://localhost:55000/agents?agents_list=002,003&pretty=true&select=version" -H  "Authorization: Bearer $TOKEN"

check your wazuh dashboard - agents, the 5 pods of daemonset wazuh agents version should have upgraded, in my case, wazuh agent version is 4.6.0

source :

https://documentation.wazuh.com/current/user-manual/agents/remove-agents/restful-api-remove.html

https://documentation.wazuh.com/current/user-manual/agents/remote-upgrading/upgrading-agent.html

chatgpt :) 

my hub.docker.com repository :

https://hub.docker.com/repository/docker/halilintar8/wazuh-agent3/general


enjoy :) 


