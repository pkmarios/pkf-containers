# Drivers for Hardware Security Modules (HSMs)

When working with HSMs many organizations already have HSMs in place, in many cases with teams that are experts in managing these HSMs. The HSM PKCS#11 drivers used may be specific approved versions, well tested with the HSM firmware version deployed. Therefore it is hard to include the correct drivers for specific users by default in containers. In addition HSM drivers are distributed under license from the manufacturer, meaning that we may not be allowed to re-distribute drivers integrated with our containers.

With the above considerations, there is a need to be able to add specific HSM drivers to pre-packaged containers in an easy way.

## Adding HSM drivers on top of the EJBCA container

The example Dockerfile adds a file system layer with the relevant HSM drivers and configuration on top of the EJBCA container. This is an easy way to add and use PKCS#11 drivers that do not require a running daemon in the container.

## HSM driver location
The example also shows how to add the driver location, as well as set some other properties, to the list that EJBCA looks for if the driver location is not already by default by EJBCA. 
For most HSMs the common driver locations is already known, check conf/web.properties.sample for the locations that EJBCA already searches by default.

## Usage

The example Dockerfile adds PKCS#11 drivers and confiugration for the (FIPS and CC EN 419221-5 certified) [https://www.i4p.com/](Trident HSM), created by I4P. 

The Dockerfile can be modified to add drivers of your choice.

TODO: add command line example how to run docker-compose to build and run the container.

# Docker containers

Enterprise Edition containers of PrimeKey applications are made available to customers from a PrimeKey registry. Visit https://www.primekey.com/ for more information.

Community Edition containers of PrimeKey applications are made available on Dockerhub. See [https://hub.docker.com/r/primekey/](https://hub.docker.com/r/primekey/) for a list of currently published applications.
