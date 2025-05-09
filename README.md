# Enforcing IntelliJ Code Style with Spotless in a Maven Project

Maintaining a consistent code style is crucial in any Java project, especially when working in teams or across long-lived repositories. While IntelliJ IDEA provides excellent formatting capabilities, ensuring all contributors follow the same style can be challenging. That’s where **Spotless**, a powerful code formatter plugin, comes in.

In this guide, we’ll configure Spotless in a **Maven** project to use **IntelliJ’s default Java formatting style**, by leveraging the Eclipse formatter that closely matches IntelliJ’s output.

---

## Why Spotless?

- Automatic formatting with `mvn spotless:apply`
- Can be enforced in CI/CD to fail builds on style violations
- Works with Maven and Gradle
- Supports multiple formatter engines (Google, Eclipse, Prettier, etc.)

---

## Step 1: Export IntelliJ Java Formatting Style

IntelliJ’s default formatter is not natively supported in Spotless. However, we can export an **Eclipse-compatible XML format** that mimics IntelliJ's style:

1. Open **IntelliJ IDEA**
2. Go to **Preferences (macOS)** or **Settings (Windows/Linux)**
3. Navigate to: `Editor` → `Code Style` → `Java`
4. Click the gear icon (next to the Scheme dropdown) → **Export** → **Eclipse XML Profile**
5. Save the file as `intellij-style.xml` in your project root

---

## Step 2: Configure Spotless in Maven

Add the following plugin configuration to your `pom.xml`:

```xml
<plugin>
    <groupId>com.diffplug.spotless</groupId>
    <artifactId>spotless-maven-plugin</artifactId>
    <version>2.45.0</version>
    <executions>
        <execution>
            <id>format</id>
            <phase>validate</phase>
            <goals>
                <goal>apply</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <java>
            <eclipse>
                <file>${project.basedir}/intellij-style.xml</file>
            </eclipse>
            <includes>
                <include>src/main/java/**/*.java</include>
                <include>src/test/java/**/*.java</include>
            </includes>
            <trimTrailingWhitespace>true</trimTrailingWhitespace>
            <endWithNewline>true</endWithNewline>
            <indentWithSpaces>true</indentWithSpaces>
        </java>
    </configuration>
</plugin>

