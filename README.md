# aro-hello-chris-spoke-hub

export AZR_RESOURCE_LOCATION=eastus
export AZR_RESOURCE_GROUP=cnuland-ngc-openshift-private
export AZR_CLUSTER=cnuland-private-cluster
export AZR_PULL_SECRET=~/Downloads/pull-secret.txt
export NETWORK_SUBNET=10.0.0.0/20
export CONTROL_SUBNET=10.0.0.0/24
export MACHINE_SUBNET=10.0.1.0/24
export FIREWALL_SUBNET=10.0.2.0/24
export JUMPHOST_SUBNET=10.0.3.0/24

```
JUMP_IP=$(az vm list-ip-addresses -g $AZR_RESOURCE_GROUP -n jumphost -o tsv \
--query '[].virtualMachine.network.publicIpAddresses[0].ipAddress')
echo $JUMP_IP
```
az network firewall application-rule create -g $AZR_RESOURCE_GROUP -f aro-private     \
    --collection-name 'Allow_Egress'                                                  \
    --action allow                                                                    \
    --priority 100                                                                    \
    -n 'required'                                                                     \
    --source-addresses '*'                                                            \
    --protocols 'http=80' 'https=443'                                                 \
    --target-fqdns '*.quay.io' 'registry.redhat.io', '*.redhat.com'

    az network firewall application-rule create -g $AZR_RESOURCE_GROUP -f aro-private     \
    --collection-name 'Azure'                                                  \
    --action allow                                                                    \
    --priority 100                                                                    \
    -n 'required'                                                                     \
    --source-addresses '*'                                                            \
    --protocols 'http=80' 'https=443'                                                 \
    --target-fqdns 'management.azure.com' '*.blob.core.windows.net', 'login.microsoftonline.com'
```
