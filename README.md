[![license](http://img.shields.io/badge/license-MIT-brightgreen.svg)](https://github.com/luisespinal/xfmt-maven-plugin/blob/master/LICENSE)

## xfmt-maven-plugin

This is a fork I made off [coveo/fmt-maven-plugin](https://github.com/coveo/fmt-maven-plugin) so that it works on Java 1.7 and to make aosp the default style.

I also changed the groupId and artifactId since I consider this a different plugin from the original. The requirement to have it working under 1.7 makes it impossible to contribute that change back to the parent fork. So we might as well use different Maven coordinates.

There are other changes I intend to make that will further deviate this from the original fork. With that said, I'm glad and thankful that good people have done such an amount of work in the original plugin under such a liberal license (MIT). Without them, this experiment would not be possible

I hope you find this "deviant" plugin useful :) 

Original package names remain the same (to acknowledge original authors.)

## Configuration

You could do someting like this:

            <properties>
                <outputDirectory>${project.build.directory}/generated-sources</outputDirectory>
            </properties>
            <build>
                ...
                <plugin>
                    <groupId>io.github.luisespinal</groupId>
                    <artifactId>xfmt-maven-plugin</artifactId>
                    <version>1.0.0-M1</version>
                    <executions>
                        <execution>
                            <id>apply-code-style</id>
                            <phase>generate-sources</phase>
                            <!--
                            Or "process-sources" if you want to format
                            generated sources.
                            -->
                            <goals>
                                <goal>format</goal>
                            </goals>
                        </execution>
                        <execution>
                            <id>validate-code-style</id>
                            <phase>validate</phase>
                            <goals>
                                <goal>check</goal>
                            </goals>
                        </execution>
                    </executions>
                    <configuration>
                        <additionalSourceDirectories>
                            <!-- you will need this if
                                 you want to format generated
                                 sources
                             -->
                            <param>${outputDirectory}</param>
                        </additionalSourceDirectories>
                        <style>aosp</style><!--  the default -->
                    </configuration>
                </plugin>
                
                ...
                
            </build>



## fmt-maven-plugin (Original content from parent project, from where this project was forked from)

Formats your code using [google-java-format](https://github.com/google/google-java-format) which follows [Google's code styleguide](https://google.github.io/styleguide/javaguide.html).

The format cannot be configured by design.

If you want your IDE to stick to the same format, check out [the available configuration files](https://github.com/google/styleguide)

* [Eclipse](https://github.com/google/styleguide/blob/gh-pages/eclipse-java-google-style.xml)
* [IntelliJ IDEA](https://github.com/google/styleguide/blob/gh-pages/intellij-java-google-style.xml)

## Usage

### Standard pom.xml

To have your sources automatically formatted on each build, add to your pom.xml:

```xml
    <build>
        <plugins>
            <plugin>
                <groupId>com.coveo</groupId>
                <artifactId>fmt-maven-plugin</artifactId>
                <version>2.8</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>format</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

If you prefer, you can only check formatting at build time using the `check` goal:

```xml
    <build>
        <plugins>
            <plugin>
                <groupId>com.coveo</groupId>
                <artifactId>fmt-maven-plugin</artifactId>
                <version>2.8</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>check</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

### Options

`sourceDirectory` represents the directory where your Java sources that need to be formatted are contained. It defaults to `${project.build.sourceDirectory}`

`testSourceDirectory` represents the directory where your test's Java sources that need to be formatted are contained. It defaults to `${project.build.testSourceDirectory}`

`additionalSourceDirectories` represents a list of additional directories that contains Java sources that need to be formatted. It defaults to an empty list.

`verbose` is whether the plugin should print a line for every file that is being formatted. It defaults to `false`.

`filesNamePattern` represents the pattern that filters files to format. The defaults value is set to `.*\.java`.

`skip` is whether the plugin should skip the operation.

`skipSortingImports` is whether the plugin should skip sorting imports.

`style` sets the formatter style to be _google_ or _aosp_. By default this is 'google'. Projects using Android conventions may prefer `aosp`.

example:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>com.coveo</groupId>
            <artifactId>fmt-maven-plugin</artifactId>
            <version>2.8</version>
            <configuration>
                <sourceDirectory>some/source/directory</sourceDirectory>
                <testSourceDirectory>some/test/directory</testSourceDirectory>
                <verbose>true</verbose>
                <filesNamePattern>.*\.java</filesNamePattern>
                <additionalSourceDirectories>
                    <param>some/dir</param>
                    <param>some/other/dir</param>
                </additionalSourceDirectories>
                <skip>false</skip>
                <skipSortingImports>false</skipSortingImports>
                <style>google</style>
            </configuration>
            <executions>
                <execution>
                    <goals>
                        <goal>format</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```



### check Options

`displayFiles` default = true. Display the list of the files that are not compliant

`displayLimit` default = 100. Number of files to display that are not compliant`

`style` sets the formatter style to be _google_ or _aosp_. By default this is 'google'. Projects using Android conventions may prefer `aosp`.

example to not display the non-compliant files:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>com.coveo</groupId>
            <artifactId>fmt-maven-plugin</artifactId>
            <version>2.8</version>
            <configuration>
                <displayFiles>false</displayFiles>
            </configuration>
            <executions>
                <execution>
                    <goals>
                        <goal>check</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

example to limit the display up to 10 files
```xml
<build>
    <plugins>
        <plugin>
            <groupId>com.coveo</groupId>
            <artifactId>fmt-maven-plugin</artifactId>
            <version>2.8</version>
            <configuration>
                <displayLimit>10</displayLimit>
            </configuration>
            <executions>
                <execution>
                    <goals>
                        <goal>check</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

### Command line

You can also use it on the command line

`mvn com.coveo:fmt-maven-plugin:format`

You can pass parameters via standard `-D` syntax.
`mvn com.coveo:fmt-maven-plugin:format -Dverbose=true`

`-Dfmt.skip` is whether the plugin should skip the operation.


### Deploy

```
git tag v0.0.0

mvn versions:set -DnewVersion=0.0.0
mvn clean deploy -P release
```
