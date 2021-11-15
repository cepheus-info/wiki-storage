---
title: Publish your shared library
description: Packaging and Publish your jar to maven Central
published: true
date: 2021-11-15T19:42:20.347Z
tags: 
editor: markdown
dateCreated: 2021-11-15T19:00:10.866Z
---

## Why?
Since I realized there would be so much infrastructure codes to write before I can config an Axon based service, I decided to wrap them into a common package.

## I. Create a library project

We created a library named cepheus_axon_extension in the following repository: [https://github.com/cepheus-info/cepheus_axon_extension](https://github.com/cepheus-info/cepheus_axon_extension).

### Project Structure
```
├─.gradle
├─gradle
│  └─wrapper
└─src
    ├─commonMain
    │  ├─kotlin
    │  └─resources
    ├─commonTest
    │  ├─kotlin
    │  └─resources
    ├─jvmMain
    │  ├─kotlin
    │  │  └─info
    │  │      └─cepheus
    │  │          └─axon
    │  │              └─infrastructure
    │  │                  ├─adapter
    │  │                  ├─boundary
    │  │                  │  ├─command
    │  │                  │  └─query
    │  │                  ├─database
    │  │                  └─transaction
    │  │                      └─jta
    │  └─resources
    │      └─META-INF
    ├─jvmTest
    │  ├─kotlin
    │  └─resources
    ├─nativeMain
    │  ├─kotlin
    │  └─resources
    └─nativeTest
        ├─kotlin
        └─resources
```
### How to?
The project was created with kotlin template, but it's nothing more than a normal java project which targeted to multiple platform.
![screenshot_2021-11-16_024139.png](/axon-framework/screenshot_2021-11-16_024139.png)

The build.gradle was as below:
```groovy
import javax.naming.ConfigurationException

plugins {
    id 'java-library'
    id 'org.jetbrains.kotlin.multiplatform' version '1.5.31'
    id 'maven-publish'
    id 'signing'
}

group = 'info.cepheus'
version = '0.0.1'
archivesBaseName = 'cepheus_axon_extension'

repositories {
    mavenCentral()
}

dependencies {
    // omit dependencies...
}

kotlin {
    // omit targets
}

ext {
    it.'signing.secretKeyRingFile' = project.findProperty('signing.secretKeyRingFile')
    it.'signing.password' = project.findProperty('signing.password')
    it.'signing.keyId' = project.findProperty('signing.keyId')
    it.ossrhUsername = project.findProperty('ossrhUsername')
    it.ossrhPassword = project.findProperty('ossrhPassword')
    it.ossrhStagingProfileId = project.findProperty('ossrhStagingProfileId')
}

publishing {
    publications {
        maven(MavenPublication) {
            groupId 'info.cepheus'
            artifactId 'cepheus_axon_extension'
            version '0.0.1'
        }
    }
    repositories {
        //
        // repositories to publish to
        //
        maven {
            name 'ossrh'
            url 'https://s01.oss.sonatype.org/service/local/staging/deploy/maven2/'
            credentials {
                username = ossrhUsername
                password = ossrhPassword
            }
        }
    }
}

tasks.withType(PublishToMavenRepository) {
    doFirst {
        if (!ossrhUsername) {
            throw new ConfigurationException('Please set the sonatype username with project property "ossrhUsername" ')
        }
        if (!ossrhPassword) {
            throw new ConfigurationException('Please set the sonatype password with project property "ossrhPassword" ')
        }
    }
}
```

1. We change id 'java' to 'java-library' in the plugins section.
2. We added 'maven-publish' and 'signing' plugins.
3. We added the whole publishing section, which specified the publishing repository, and credentials info.

The ~/.gradle/gradle.properties are like below:
```properties
signing.keyId=your-key-id
signing.password=your-signing-password
signing.secretKeyRingFile=C:\\Users\\wujun\\.gnupg\\private-keys-v1.d\\some-words.key

ossrhUsername=sonatype-username
ossrhPassword=sonatype-password
ossrhStagingProfileId=sonatype-staging-profile
```

## II. Publish to mavenLocal
We can publish the jar to mavenLocal for some integration test.
There's a gradle task named publishToMavenLocal to do that.

Then in your client application, reference this package in your dependencies section, and exclude your library in remote repositories. That's because you have not publish it remotely yet, the build will fail regarding to this.

The build.gradle in your client application:
```groovy
repositories {
    mavenCentral() {
        content {
            excludeGroup("info.cepheus")
        }
    }
    mavenLocal()
}

dependencies {
		// omit other dependencies...

    implementation "info.cepheus:cepheus_axon_extension:0.0.1"
}
```

## III. Publish to mavenCentral
We can never publish to mavenCentral directly, instead, we need publishing to one of those proxyed repositories and our artifacts appear in the Central.

### 1) Sign up / Login to sonatype
First, [Create a Jira account](https://issues.sonatype.org/secure/Signup!default.jspa) if not yet.

Second, [Create a New Project ticket](https://issues.sonatype.org/secure/CreateIssue.jspa?issuetype=21&pid=10134) following the instructions.

Above steps can be found in the guide from [https://central.sonatype.org/publish/publish-guide/](https://central.sonatype.org/publish/publish-guide/).

### 2) Verify your domain name via TXT records
After created the New Project Issue, we can find the guidelines to verify the domain name or using Github prefixed groupId alternatively.
Note, You should create your TXT record with empty Hostname field.
And after created you might need to reopen the issue for further changes taken place.

### 3) GPG key
Generate a gpg key pair if not yet:
```bash
gpg2 --full-generate-key
```
Select 4096 bits long and leave other items default.
```bash
gpg: directory '/home/wujuntao/.gnupg' created
gpg: keybox '/home/wujuntao/.gnupg/pubring.kbx' created
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 4096
Requested keysize is 4096 bits
Please specify how long the key should be valid.

GnuPG needs to construct a user ID to identify your key.

You selected this USER-ID:
    "Juntao Wu (wujuntaocn) <wujuntaocn@outlook.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
gpg: /home/wujuntao/.gnupg/trustdb.gpg: trustdb created
gpg: key 596526528E1AD2A8 marked as ultimately trusted
gpg: directory '/home/wujuntao/.gnupg/openpgp-revocs.d' created
gpg: revocation certificate stored as '/home/wujuntao/.gnupg/openpgp-revocs.d/8EE94F34CE9218CC8393020F596526528E1AD2A8.rev'
public and secret key created and signed.

pub   rsa4096 2021-11-15 [SC]
      8EE94F34CE9218CC8393020F596526528E1AD2A8
uid   Juntao Wu (wujuntaocn) <wujuntaocn@outlook.com>
sub   rsa4096 2021-11-15 [E]
```
Export the public parts into a text file:
```bash
gpg2 --armor --export 8EE94F34CE9218CC8393020F596526528E1AD2A8 > public.pgp
```

Upload the public key to the well known pgp sites such as:
[https://keys.openpgp.org](https://keys.openpgp.org)
[https://keyserver.ubuntu.com](https://keyserver.ubuntu.com)

Export the secring into a file:
```bash
gpg2 --export-secret-keys -o secring.key
```

### 4) Modify ~/.gradle/gradle.properties for correct credentials

Create or update your global gradle.properties file rather than the project local properties. This is because we often add the gradle.properties into source control, which is risky for credentials.

### 5) Publish and check availabilities

Navigate to [https://s01.oss.sonatype.org/](https://s01.oss.sonatype.org/) to check if the publication successfully.

## IV. Check package availability

Remove your exclusion in client build.gradle, now it's time to check if the jar is publicly available.

You can further check other ways for sharing company-wide library packages.
