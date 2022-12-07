# Adding Hardware Security Modules (HSMs) drivers to another file system layer

By taking a release container and adding the PKCS#11 `.so` to the image, you can enable use of the HSM driver.

## Adding HSM drivers on top of the EJBCA container using docker-compose

The example Dockerfile adds a file system layer with the relevant HSM drivers and configuration on top of the EJBCA container. This is an easy way to add and use PKCS#11 drivers that do not require a running daemon in the container.

## HSM driver location
The example also shows how to add the driver location, as well as set some other properties, to the list that EJBCA looks for if the driver location is not already by default by EJBCA. 
For most HSMs the common driver locations is already known, check conf/web.properties.sample for the locations that EJBCA already searches by default.

## Usage

The example Dockerfile adds PKCS#11 drivers and confiugration for the (FIPS and CC EN 419221-5 certified) [Trident HSM](https://www.i4p.com/), created by I4P. 

The Dockerfile can be modified to add drivers of your choice.

TODO: add command line example how to run docker-compose to build and run the container.

## Adding HSM drivers on top of the EJBCA container using file system mounts

Another way to add relevant HSM drivers, along with their configuration, is by using docker volume mounts.

For example to add a Thales DPoD client (as described in the EJBCA documentation) you can simply mount it and run the container.

In EJBCA before 7.5.0:

```
sudo mkdir -p /opt/primekey/ejbca/conf
 
echo "httpserver.pubhttp=80
httpserver.pubhttps=443
httpserver.privhttps=443
httpserver.external.privhttps=443
web.reqcertindb=false
 
#cryptotoken.p11.lib.114.file=/opt/primekey/cloudhsm/lib/libliquidsec_pkcs11.so
 
cryptotoken.p11.lib.255.name=P11 Proxy
cryptotoken.p11.lib.255.file=/opt/primekey/p11proxy-client/p11proxy-client.so
cryptotoken.p11.lib.255.canGenerateKeyMsg=ClientToolBox must be used to generate keys for this HSM provider.
# Normally key generation will be allowed via the UI
cryptotoken.p11.lib.255.canGenerateKey=true
 
# Enable usage of Azure Key Vault Crypto Token in the Admin UI
keyvault.cryptotoken.enabled=true
 
# Enable usage of AWS KMS Crypto Token in the Admin UI
awskms.cryptotoken.enabled=true
web.docbaseuri=disabled
 
cryptotoken.p11.lib.24.name=Thales DPoD
cryptotoken.p11.lib.24.file=/opt/thales/dpodclient/libs/64/libCryptoki2.so
" | sudo tee -a "/opt/primekey/ejbca/conf/web.properties"
```

Start the container, adding the file system mounts and the environment variable to point to the driver location:

```
sudo docker run -it --rm --name thales_test -p 80:8080 -p 443:8443 -v /opt/primekey/ejbca/conf/web.properties:/etc/ejbca/conf/web.properties -v /opt/thales:/opt/thales -e ChrystokiConfigurationPath=/opt/thales/dpodclient registry.primekey.se/primekey/ejbca-ee:7.4.3-5
```

Manually check that files have been mounted correctly:

```
sudo docker ps
sudo docker exec -ti thales_test /bin/bash
ls -al /opt/thales
```

# EJBCA Docker containers

Enterprise Edition containers of Keyfactor EJBCA applications are made available to customers from a Keyfactor registry. Visit https://www.keyfactor.com/ for more information.

Community Edition containers of Keyfactor EJBCA applications are made available on Dockerhub. See [https://hub.docker.com/r/keyfactor/](https://hub.docker.com/r/keyfactor/ejbca-ce) for a list of currently published applications.
