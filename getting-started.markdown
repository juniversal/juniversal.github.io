---
layout: getting-started
title: Getting Started
permalink: /getting-started/
---

### Getting source and building ###

Soon, you'll be able to grab builds from our CI server or Maven Central.
But for now, it's best to build yourself (which is pretty easy).

Get the source.

    git clone https://github.com/juniversal/juniversal.git
    git clone https://github.com/juniversal/jsimple.git

Build the translator.   The Gradle wrapper downloads Gradle automatically---no need to install any tools.

    cd juniversal
    ./gradlew build install        [Linux/Mac/Cygwin]
    gradlew build install          [Windows]

`install` above means to copy the built stuff to your local Maven repository (`<home directory>/.m2` by default), where subsequent build steps will find it.

Next build the Gradle plugins.

    cd juniversal/build-tool-plugins/juniversal-gradle-plugins
    ./gradlew build install        [Linux/Mac/Cygwin]
    gradlew build install          [Windows]

Build the JSimple Java libraries and translate them to C#.

    cd jsimple
    ./gradlew build build install javaToCSharp   [Linux/Mac/Cygwin]
    gradlew build install javaToCSharp           [Windows]

Optionally, you can build all the JSimple C# projects by opening `jsimple/c#/jsimple.sln` in Visual Studio.
But you can also build them by just pointing directly, from your app's own Visual Studio file, to the JSimple modules you use.

### Running the translator(s) the first time ###

As an example, say you're working on Acme Corp's spiffy new mobile app.  You've got an MVC design & figure the acme-model.jar model component (which may include persistent state, business logic, and client/server comms) is a good one to try make cross platform with JUniversal.

You probably start with a directory layout like:

    acme-model/src/main/java                   [Java source]
    acme-model/src/test/java                   [Java JUnit tests source]

The easiest way to run the translator is with our Gradle plugins.   Add the following to your project's `build.gradle` file to JUniversal-enable it:

	buildscript {
	    repositories {
	        mavenLocal()
	    }

	    dependencies {
	        classpath group: 'org.juniversal', name: 'juniversal-gradle-plugins', version: '0.8-SNAPSHOT'
	    }
	}

    apply plugin: 'javaToCSharp'

	javaToCSharp {
	}

`mavenLocal()` above means to look in the local Maven repository directory, which is where the JUniversal stuff was copied when built above.

To invoke the translator then run:

    gradle javaToCSharp

It will translate the main and (if present) test source.   JUnit tests are changed to NUnit in C# (see below).

To also translate to Objective-C, include this in your `build.gradle` file:

    apply plugin: 'javaToObjectiveC'

    javaToObjectiveC {
    }

Download j2objc from http://github.com/google/j2objc/releases and set the `J2OBJC_HOME` environement variable to point to the j2objc distro, so our Gradle plugin can find it.   Then run `gradle javaToObjectiveC` to translate to objective C via `j2objc`.

For fun, you can even translate to C++ if you want to see the current state of our work in progress C++ translator:

    apply plugin: 'javaToCPlusPlus'

    javaToCPlusPlus {
    }

Run `gradle javaToCPlusPlus` to do the translation.   You'll see a lot of C++ code get generated, but it's only partially baked, so don't expect it to work.

We'll write up more later about using j2objc and JUniversal, but the rest of this doc will just cover C#.

### Making your source translator friendly ###

Now (after fixing up the Java for any translator reported issues) you'll have directory structure like:

    acme-model/src/main/java                   [Java source; cross platform code]
    acme-model/src/test/java                   [Java cross JUnit tests source;  cross platform code]
    acme-model/c#                              [C# translated source]
    acme-model/c#-test                         [C# translated test source]

The C# code is all generated.

The Visual Studio project file you have to create yourself though:  
Launch Visual Studio and create a new (empty) project via the File / New Project wizard.
You can create whatever project type you want, but Portable Class Library is often a good choice.
One nice thing about using JUniversal with Windows is that JUniversal C# libraries tend
to have all their platform dependencies segregated out, so you can normally use a
Portable Class Library that works across all flavors on Windows--desktop, phone, tablet, even XBox.


After creating the project, you'll get a .vcproj generated for the project and Properties/AssemblyInfo.cs file.
Normal practice in Windows, much like Java, is that module unit tests are put in a separate project.
So go through the wizard again, creating another project for acme-model-test.

Close Visual Studio & move those files if necessary to create a directory structure like:

    acme-model/src/main/java                   [Java source; cross platform]
    acme-model/src/test/java                   [Java cross JUnit tests source; cross platform]
    acme-model/src/main/java-nontranslated     [Optional; any platform specific Java source]
    acme-model/c#                              [acme-model C# project code; most/all generated by translator]
    acme-model/c#/acme-model.vcproj            [acme-model C# project file]
    acme-model/c#/Properties/AssemblyInfo.cs   [acme-model assembly info file]

    acme-model/src/main/java                   [Java source; cross platform]
    acme-model/src/test/java                   [Java cross JUnit tests source; cross platform]
    acme-model/src/main/java-nontranslated     [Optional; any platform specific Java source]
    acme-model/c#                              [acme-model C# project code; most/all generated by translator]
    acme-model/c#/acme-model.vcproj            [acme-model C# project file]
    acme-model/c#/Properties		           [acme-model C# project file]


The typical approach on Windows is for the tests to be in a separate assembly.  


To build on Windows, you can build directly in Visual Studio (handy when developing), build the entire 


That is, the Java stuff is layed out in the normal way.   Maven directory conventions are used above, but you can use whatever you prefer.

    acme-model/src/main/java                   [Java source; cross platform]
    acme-model/src/test/java                   [Java cross JUnit tests source; cross platform]
    acme-model/src/main/java-nontranslated     [Optional; any platform specific Java source]
    acme-model/c#                              [acme-model C# project code; most/all generated by translator]
    acme-model/c#/acme-model.vcproj            [acme-model C# project file]
    acme-model/c#/nontranslated                [Optional; any platform specific C# source]
    acme-model/c#-test                         [acme-model C# project code; most/all generated by translator]
    acme-model/c#/acme-model.vcproj            [acme-model C# project file]
    acme-model/c#/nontranslated                [Optional; any platform specific C# source]


The C# directory structure is also pretty conventional, for a C# project.






As for the test code, JUnit tests are translated to NUnit tests in C#.   We're considering enhancing that to also
support MSTest or XUnit, but current support is NUnit.  The idea is that you can use JUnit test runner, in your IDE or build
scripts, to run your Java tests.   And, on the C# side, can use NUnit to run the C# unit tests,
from Visual Studio (e.g. NUnit extension or Resharper) or outside. 

To tweak your unit tests to be translation friendly, do this:
Change your JUnit test classes to derive from the jsimple.unit.UnitTest class in the jsimple-unit module.
With that, the assert methods typically stay exactly the same as they are in your current code, but they'll change
from static calls to calls to wrapper methods in the UnitTest class (with the same names).   The @Test annotations stay the same.
Any test setup code should move to a setUp method override, instead of using the @SetUp annotation.












> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
> 
> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
> id sem consectetuer libero luctus adipiscing.

> This is the first level of quoting.
>
> > This is nested blockquote.
>
> Back to the first level.

Here is an example of AppleScript:

    tell application "acme-model"
        beep
    end tell



