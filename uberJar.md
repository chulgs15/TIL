* https://docs.gradle.org/current/userguide/working_with_files.html#sec:creating_uber_jar_example
  * fat jar 를 만들기 위한 공식 문서



In the Java space, applications and their dependencies typically used to be packaged as separate JARs within a single distribution archive. That still happens, but there is another approach that is now common: placing the classes and resources of the dependencies directly into the application JAR, creating what is known as an uber or fat JAR.

종속성 클래스와 리소스를 하나의 Application Jar 로 만드는 방법을 찾고 있다. 

uber or fat jar 방식이다.

Gradle makes this approach easy to accomplish. Consider the aim: to copy the contents of other JAR files into the application JAR. All you need for this is the [Project.zipTree(java.lang.Object)](https://docs.gradle.org/current/dsl/org.gradle.api.Project.html#org.gradle.api.Project:zipTree(java.lang.Object)) method and the [Jar](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.bundling.Jar.html) task, as demonstrated by the `uberJar` task in the following example: