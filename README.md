# gf-basic-security-manager
## Prerequirement
- Make [Pivotal
Commercial Maven Repository](https://commercial-repo.pivotal.io/) account
- Add your account in `$HOME/.m2/settings.xml` as follows.
```xml
   <settings>
       <servers>
           <server>
               <id>gemfire-release-repo</id>
               <username>MY-USERNAME@example.com</username>
               <password>MY-PLAINTEXT-PASSWORD</password>
           </server>
       </servers>
   </settings>
```

## How to build
   `mvn clean package`

## gemfire.properties
   `security-manager=com.vmware.gemfire.BasicSecurityManager`
   
   `security-peer-auth-init=com.vmware.gemfire.UserPasswordAuthInit`

## Start locator01

`gfsh>start locator --name=pocLocator1 --locators=<LOCATOR_HOST01>[10334],LOCATOR_HOST02[10334],LOCATOR_HOST03[10334] --initial-heap=32g --max-heap=32g --J=-Dgemfire.securitymanager=com.vmware.gemfire.BasicSecurityManager --classpath=/gemfire/gemfire-poc/vmware-gemfire-10.0.0/gf-basic-security-manager-1.0.jar --properties-file=/gemfire/gemfire-poc/vmware-gemfire-10.0.0/gemfire.properties`

## Start locator02

`gfsh>connect --locator=LOCATOR_HOST01[10334] --user=gemfire --password=changeme`

`gfsh>start locator --name=pocLocator2 --locators=LOCATOR_HOST01[10334],LOCATOR_HOST02[10334],LOCATOR_HOST03[10334] --initial-heap=32g --max-heap=32g --J=-Dgemfire.securitymanager=com.vmware.gemfire.BasicSecurityManager --classpath=/gemfire/gemfire-poc/vmware-gemfire-10.0.0/gf-basic-security-manager-1.0.jar --properties-file=/gemfire/gemfire-poc/vmware-gemfire-10.0.0/gemfire.properties`

## Start locator03

`gfsh>connect --locator=LOCATOR_HOST01[10334] --user=gemfire --password=changeme`
 
`gfsh>start locator --name=pocLocator3 --locators=LOCATOR_HOST01[10334],LOCATOR_HOST02[10334],LOCATOR_HOST03[10334] --initial-heap=32g --max-heap=32g --J=-Dgemfire.securitymanager=com.vmware.gemfire.BasicSecurityManager --classpath=/gemfire/gemfire-poc/vmware-gemfire-10.0.0/gf-basic-security-manager-1.0.jar --properties-file=/gemfire/gemfire-poc/vmware-gemfire-10.0.0/gemfire.properties`

## Start server1

`gfsh>connect --locator=LOCATOR_HOST01[10334] --user=gemfire --password=changeme`
 
`gfsh> start server --name=pocServer1 --locators=LOCATOR_HOST01[10334],LOCATOR_HOST02[10334],LOCATOR_HOST03[10334] --initial-heap=48g --max-heap=48g --classpath=/gemfire/gemfire-poc/vmware-gemfire-10.0.0/gf-basic-security-manager-1.0.jar --user=gemfire --password=changeme`

## Start server2

`gfsh>connect --locator=LOCATOR_HOST01[10334] --user=gemfire --password=changeme`
 
`gfsh>start server --name=pocServer2 --locators=LOCATOR_HOST01[10334],LOCATOR_HOST02[10334],LOCATOR_HOST03[10334] --initial-heap=48g --max-heap=48g --classpath=/gemfire/gemfire-poc/vmware-gemfire-10.0.0/gf-basic-security-manager-1.0.jar --user=gemfire --password=changeme`
 

## Start other servers if required....

## create partition region

`gfsh>create region --name=testRegion --type=PARTITION`


## run the client to ingest the data on secure cluster

`[gemfire@xxxxxxxx]$ java -classpath gf-basic-security-manager-1.0.jar:./lib/* com.vmware.gemfire.CacheClient`

## Pulse UI

`http://<LOCATOR_HOST01>:7070/pulse/login.html`

