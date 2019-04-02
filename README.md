# Proteus RSocket Bill Of Material Project

A set of compatible versions for all these projects is curated under a BOM ("Bill of Material").

## Using the BOM with Maven
In Maven, you need to import the bom first:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.netifi.proteus</groupId>
            <artifactId>rsocket-bom</artifactId>
            <version>1.6.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```
Notice we use the `<dependencyManagement>` section and the `import` scope.

Next, add your dependencies to the relevant rsocket projects as usual, except without a `<version>`:

```xml
<dependencies>
    <dependency>
        <groupId>io.rsocket</groupId>
        <artifactId>rsocket-core</artifactId>
    </dependency>
    <dependency>
        <groupId>io.rsocket</groupId>
        <artifactId>rsocket-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

## Using the BOM with Gradle
### Gradle 5.0+
Use the `platform` keyword to import the Maven BOM within the `dependencies` block, then add dependencies to
your project without a version number.

```groovy
dependencies {
     // import BOM
     implementation platform('io.rsocket:rsocket-bom:Palladium-RELEASE')

     // add dependencies without a version number
     implementation 'io.rsocket:rsocket-core'
}
```

### Gradle 4.x and earlier
Gradle versions prior to 5.0 have no core support for Maven BOMs, but you can use Spring's [`gradle-dependency-management` plugin](https://github.com/spring-gradle-plugins/dependency-management-plugin).

First, apply the plugin from Gradle Plugin Portal (check and change the version if a new one has been released):

```groovy
plugins {
    id "io.spring.dependency-management" version "1.0.7.RELEASE"
}
```
Then use it to import the BOM:

```groovy
dependencyManagement {
     imports {
          mavenBom "io.rsocket:rsocket-bom:Palladium-RELEASE"
     }
}
```

Then add a dependency to your project without a version number:

```groovy
dependencies {
     compile 'io.rsocket:rsocket-core'
}
```


## BOM Versioning Scheme
The BOM can be imported in Maven, which will provide a set of default artifact versions to use whenever the corresponding dependency is added to a pom without an explicitly provided version.

### Snapshot Artifacts

While Stable Releases are synchronized with Maven Central, fresh snapshot and milestone artifacts are provided in the _repo.spring.io_ repositories.

To add this repo to your Maven build, add it to the `<repositories>` section like the following:

```xml
<repositories>
	<repository>
	    <id>rsocket-snapshot</id>
	    <name>RSocket Snapshot Repository</name>
	    <url>https://dl.bintray.com/netifi/netifi-oss</url>
	    <snapshots>
	        <enabled>true</enabled>
	    </snapshots>
	</repository>
</repositories>
```

To add it to your Gradle build, use the `repositories` configuration like this:
```groovy
repositories {
	maven { url 'https://dl.bintray.com/netifi/netifi-oss' }
	mavenCentral()
}
```

You should then be able to import a `BUILD-SNAPSHOT` version of the BOM, like `Palladium-BUILD-SNAPSHOT`.

_Sponsored by [Netifi, Inc](https://www.netifi.com)_
