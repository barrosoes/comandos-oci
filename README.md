# comandosoci
Lista de comando OCI CLI usado em cliente para manutenção

-------------- OCI CLI COMMANDS -----------


--LISTAR BUCKETS
oci os bucket list --compartment-id ocid1.compartment.oc1..aaaaaaaa...

--LISTAR USERS
oci iam user list --query 'data[*].{"name":"name","description":"description"}'

--LISTAR OBJETOS DO BUCKET
oci os object list --bucket-name bkp-db-inbrands |grep -i name

--LISTAR OBJETOS DO BUCKET POR BUCKET
oci os object list --bucket-name bkp-db-inbrands |grep -i name |grep -i nome-bucket

--LISTAR STATUS DB SYSTEM
ociCompartmentList=$(oci iam compartment list --compartment-id ocid1.tenancy.oc1..aaaaaaaa...
for c in $(echo "$ociCompartmentList" | jq '.data | keys | .[]')
do
 compartment_ocid=$(echo "$ociCompartmentList" | jq -r ".data[$c].\"id\"")
 ocidbList=$(oci db database list -c $compartment_ocid)
 for i in $(echo "$ocidbList" | jq '.data | keys | .[]')
 do
	ocidbnodeList=$(oci db node list -c $compartment_ocid --db-system-id $(echo $ocidbList | jq -r ".data[$i].\"db-system-id\""))
	hostname=$(echo "$ocidbnodeList" | jq -r ".data[].\"hostname\"")
	lifecycle=$(echo "$ocidbnodeList" | jq -r ".data[].\"lifecycle-state\"")
	echo "Hostname: $hostname | LifeCycle: $lifecycle"
 done
done

--LISTAR NODES DO BANCO
oci db node list --compartment-id ocid1.tenancy.oc1..aaaaaaa... --db-system-id ocid1.dbsystem.oc1.sa-saopaulo-1.abtxeljrtzpped5xaheweqk2c5blumk...

-- LISTAR COMPARTMENTS
oci iam compartment list -c ocid1.tenancy.oc1..aaaaaaaa...

--LISTAR VCN
oci network vcn list -c ocid1.compartment.oc1..aaaaaaaa...

--STOP NODE BANCO ESPECIFICO
oci db node stop --db-node-id ocid1.dbnode.oc1.sa-saopaulo-1.abtxeljr52un3ssc...


--START NODE BANCO ESPECIFICO
oci db node start --db-node-id ocid1.dbnode.oc1.sa-saopaulo-1.abtxeljr52un3ssc...



copy_rman_obj.sh
#!/bin/bash
#Execução de cópia de arquivos para bucket bkp-db-xxx em Object Storage
#Anderson Barroso
COMMAND="oci os object put -bn"
BUCKET="bkp-db-xxx"
DIR="ocdb"
for x in bkp*; do  
ls -l $x |awk '{print $9}'; 
$COMMAND $BUCKET --name $DIR/$x --file $x
done
echo "Copia finalizada com sucesso!"



-- A LISTA SERA ATUALIZADA

