# Application Class-Data Sharing (AppCDS) Demo

A minimal application for demostrating application class-data sharing (AppCDS). This repository is referenced from the following HappyCoders article on new features in Java 10:

* English: [Java 10 Features (With Examples)](https://www.happycoders.eu/java/java-10-features/)
* German: [Java 10 Features (mit Beispielen)](https://www.happycoders.eu/de/java/java-10-features-de/)

You need at least Java 10, preferably OpenJDK, since AppCDS is a commercial feature in the Oracle JDK.

To run the application using AppCDS, follow these steps:

## Step 1

Compile and package the code.

```
javac -d target/classes src/eu/happycoders/appcds/*.java
jar cf target/helloworld.jar -C target/classes .
```

## Step 2

Create a list of classes that the application uses.

(On Windows, you must omit the backslashes and write everything on one line.)

```
java -Xshare:off -XX:+UseAppCDS \
    -XX:DumpLoadedClassList=helloworld.lst \
    -cp target/helloworld.jar eu.happycoders.appcds.Main
```

As the program is executed to created the class list, you should see a "Hello world!".

## Step 3

Create a JSA file (Java shared archive) from the class list.

```
java -Xshare:dump -XX:+UseAppCDS \
    -XX:SharedClassListFile=helloworld.lst \
    -XX:SharedArchiveFile=helloworld.jsa \
    -cp target/helloworld.jar
```

## Step 4

Run the application with the AppCDS activated.

```
java -Xshare:on -XX:+UseAppCDS \
    -XX:SharedArchiveFile=helloworld.jsa \
    -cp target/helloworld.jar eu.happycoders.appcds.Main
```

You should see "Hello world!" again.