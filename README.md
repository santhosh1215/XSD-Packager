This is a simple CLI tool to export several **.xsd** generated ISOXML packages into one **.jar** file for importing into your other projects.

***

__Pre-setup__

For both Linux and Windows users (Windows Linux Subsystem not supportted), run command line `xjc -version` to check if you have **xjc** installed.

On successful install, you will see

```
xjc 2.3.0-b170531.0717
```
and can skip this section.

On unsuccessful install, you can make sure you have **java** installed in your environment, this can be checked using command line `java -version`. **xjc.exe** comes as part of your jdk.

***

__Create JAR from .xsd files__

1. Clone this repository using `git clone https://github.com/dlworldpeace/ISO-Packager.git` at yout preferred storage location.
2. Add your new **.xsd** files into the `src/` directory along all other existing **.xsd** files and `main/`.
3. If your newly added .xsd files have other imports, ensure these imports exist in the same directory and their `"schemaLocation"` of the import fields are updated accordingly, for example in `citiBalanceInquiry.xsd`

change from

```
   <xsd:import namespace="urn:iso:std:iso:20022:tech:xsd:camt.052.001.02"
                schemaLocation="citiconnect/iso/camt05200102.xsd"/>
```
to the following
```
   <xsd:import namespace="urn:iso:std:iso:20022:tech:xsd:camt.052.001.02"
                schemaLocation="camt05200102.xsd"/>
```
because the the required import file `camt05200102.xsd` is in the same directory.

4. From the `src` level directory, run command `parse_xsd.sh` (Make sure you have set up your python environment) to generate all the **.java** from **.xsd** at `src/main/java/deskera/fintech/` and then export the entire project from `deskera.fintech.*` as a **.jar** package.

5. Retrieve the generated **.jar** from `target/deskera-fintech-1.0.jar`.

***

__To update JAR name or version__

If you want to change the name and version of the `.jar` exported, you can change them at `<artifactId>deskera-fintech</artifactId>` and `<version>1.0</version>` of `pom.xml` respectively before step 4 and 5 of **Create JAR from .xsd files**.

***

__To learn more__

What `parse_xsd.sh` does is a consecutive 2 steps of parsing and packaging:

1. An **xjc** command is used to parse each existing **.xsd** files into a folder of java classes, e.g. `xjc -d src/main/java -p deskera.fintech.citiBalanceInquiry src/citiBalanceInquiry.xsd -XautoNameResolution`.

2. a **maven** command `mvn pacakage` is used to export the entire java package as a **.jar** file based on the settings in `pom.xml`.
