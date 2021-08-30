![PrimeKey](../primekey_logo.png)

# Drivers for Hardware Security Module (HSM) integration

When working with HSMs many organizations already have HSMs in place, in many cases with teams that are experts in managing these HSMs. The HSM PKCS#11 drivers used may be specific approved versions, well tested with the HSM firmware version deployed. Therefore it is hard to include the correct drivers for specific users by default in containers. In addition HSM drivers are distributed under license from the manufacturer, meaning that we may not be allowed to re-distribute drivers integrated with our containers.

With the above considerations, there is a need to be able to add specific HSM drivers to pre-packaged containers in an easy way.

## Non-PKCS#11 application level drivers

EJBCA EE supports both Azure Key Vault and AWS KMS via CryptoTokens and these can be configured via the UI without any additional rebuild.

## Adding PKCS#11 driver to another file system layer

By taking a release container and adding the PKCS#11 `.so` to the image, you can enable use of the HSM driver.

This requires:
* a rebuild of the deployed image for every update of either the application or the HSM driver.
* that the HSM library works with current OS libraries in the application release
* that the HSM library works with low privileges assigned to the application image at runtime

## Using a PKCS#11 enabled hsm-driver side-car container

EE application users can leverage a proprietary module packaged as a container to invoke the HSM specific PKCS#11 library over (Pod-local) network.

This enables rolling updates of either hsm-driver or application container without the risk of application container library changes breaking the HSM driver.
