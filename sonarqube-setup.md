Installing SonarQube on macOS involves downloading and setting up the SonarQube server and ensuring that its dependencies (like Java and a database) are installed. Here's a step-by-step guide:

---

### **1. Prerequisites**
- **Java**: SonarQube requires Java 17 or higher.
- **Database**: SonarQube supports PostgreSQL, MySQL, and Oracle. PostgreSQL is recommended.
- **Memory**: At least 2 GB of RAM allocated for the SonarQube server.

---

### **2. Install Java**
1. **Check Java Version:**
   ```bash
   java -version
   ```
   If you don't have Java installed or it's an older version, install it using:
   ```bash
   brew install openjdk
   ```

2. **Add Java to PATH:**
   ```bash
   echo 'export PATH="/usr/local/opt/openjdk/bin:$PATH"' >> ~/.zshrc
   source ~/.zshrc
   ```

---

### **3. Install PostgreSQL**
1. **Install PostgreSQL using Homebrew:**
   ```bash
   brew install postgresql
   ```

2. **Start PostgreSQL:**
   ```bash
   brew services start postgresql
   ```

3. **Create a Database and User for SonarQube:**
   ```bash
   psql postgres
   ```
   Inside the PostgreSQL shell:
   ```sql
   CREATE DATABASE sonarqube;
   CREATE USER sonar WITH ENCRYPTED PASSWORD 'sonar';
   GRANT ALL PRIVILEGES ON DATABASE sonarqube TO sonar;
   \q
   ```

---

### **4. Download SonarQube**
1. **Visit the SonarQube website:**
   [SonarQube Downloads](https://www.sonarsource.com/products/sonarqube/downloads/)

2. **Download the Latest Version:**
   Choose the "Community Edition" for free usage.

3. **Extract the Downloaded Archive:**
   ```bash
   cd ~/Downloads
   tar -xvzf sonarqube-*.zip
   mv sonarqube-* /usr/local/sonarqube
   ```

---

### **5. Configure SonarQube**
1. **Navigate to the Configuration Directory:**
   ```bash
   cd /usr/local/sonarqube/conf
   ```

2. **Edit the `sonar.properties` File:**
   ```bash
   nano sonar.properties
   ```

3. **Update the Database Configuration:**
   Uncomment and update the following lines:
   ```properties
   sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube
   sonar.jdbc.username=sonar
   sonar.jdbc.password=sonar
   ```

4. **Save and Exit:**
   Press `Ctrl + O`, then `Ctrl + X`.

---

### **6. Start SonarQube**
1. **Navigate to the SonarQube Bin Directory:**
   ```bash
   cd /usr/local/sonarqube/bin/macosx-universal-64
   ```

2. **Start SonarQube:**
   ```bash
   ./sonar.sh start
   ```

3. **Check the Logs for Issues (Optional):**
   ```bash
   tail -f /usr/local/sonarqube/logs/sonar.log
   ```

---

### **7. Access SonarQube**
1. Open your browser and navigate to:
   ```
   http://localhost:9000
   ```

2. Log in using the default credentials:
   - **Username**: `admin`
   - **Password**: `admin`

3. Change the default password after the first login.
    
    - **new password**: `Sonaradmin@123`
---

### **8. Configure SonarQube Scanner**
To analyze your code, you'll need to install the SonarQube Scanner.

1. **Install SonarQube Scanner using Homebrew:**
   ```bash
   brew install sonar-scanner
   ```

2. **Verify Installation:**
   ```bash
   sonar-scanner --version
   ```

3. **Add `sonar-project.properties` to Your Project:**
   Create a `sonar-project.properties` file in your project root:
   ```properties
   sonar.projectKey=your_project_key
   sonar.host.url=http://localhost:9000
   sonar.login=your_sonar_token
   ```

4. **Run the Scanner:**
   ```bash
   sonar-scanner
   ```

---

### **9. Stop SonarQube**
To stop SonarQube:
```bash
cd /usr/local/sonarqube/bin/macosx-universal-64
./sonar.sh stop
```

---

### **Troubleshooting**
1. **Memory Issues:**
   Update `wrapper.conf` to allocate more memory:
   ```bash
   nano /usr/local/sonarqube/conf/wrapper.conf
   ```
   Modify:
   ```properties
   wrapper.java.maxmemory=2048
   ```

2. **Permissions Error:**
   Ensure the `/usr/local/sonarqube` directory has the correct permissions:
   ```bash
   sudo chown -R $(whoami):staff /usr/local/sonarqube
   ```

---


To set up a **local Spring Boot project** with **SonarQube** analysis, follow these steps:

---

### **1. Install SonarQube Locally**
Make sure SonarQube is running locally. If not already set up, follow these steps:
- Download and extract the [SonarQube Community Edition](https://www.sonarqube.org/downloads/).
- Start SonarQube:
  ```bash
  cd <sonarqube-folder>/bin/<os-folder>
  ./sonar.sh start
  ```
- Access the dashboard at: [http://localhost:9000](http://localhost:9000).

---

### **2. Add SonarQube Plugin to Your Spring Boot Project**
#### a. **Using Maven**
Add the **SonarQube plugin** in your `pom.xml`:
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.sonarsource.scanner.maven</groupId>
            <artifactId>sonar-maven-plugin</artifactId>
            <version>3.9.1.2184</version>
        </plugin>
    </plugins>
</build>
```

#### b. **Using Gradle**
If you're using Gradle, add the SonarQube plugin to your `build.gradle`:
```groovy
plugins {
    id "org.sonarqube" version "4.3.0.3225"
}
```

---

### **3. Configure SonarQube in Your Project**
#### a. **In `pom.xml` (for Maven projects)**
Add SonarQube properties:
```xml
<properties>
    <sonar.host.url>http://localhost:9000</sonar.host.url>
    <sonar.projectKey>your_project_key</sonar.projectKey>
    <sonar.login>your_sonar_token</sonar.login>
</properties>
```

#### b. **In `build.gradle` (for Gradle projects)**
Configure SonarQube settings:
```groovy
sonarqube {
    properties {
        property "sonar.projectKey", "your_project_key"
        property "sonar.host.url", "http://localhost:9000"
        property "sonar.login", "your_sonar_token"
    }
}
```

#### c. **Alternative: Use `sonar-project.properties` File**
Create a `sonar-project.properties` file in the project root:
```properties
sonar.projectKey=your_project_key
sonar.projectName=Your Spring Boot Project
sonar.projectVersion=1.0
sonar.sources=src/main/java
sonar.java.binaries=target/classes
sonar.host.url=http://localhost:9000
sonar.login=your_sonar_token
```

---

### **4. Generate SonarQube Token**
Follow the steps mentioned earlier to generate a token:
1. Go to **My Account** > **Security** in SonarQube.
2. Generate a new token.
3. Use the token in your `pom.xml`, `build.gradle`, or `sonar-project.properties`.

---

### **5. Run SonarQube Analysis**
#### a. **For Maven Projects**
Run the following Maven command to analyze your project:
```bash
mvn clean install sonar:sonar
```

#### b. **For Gradle Projects**
Run the following Gradle command:
```bash
gradle sonarqube
```

#### c. **For CLI Scanner**
Alternatively, use the SonarScanner CLI:
1. Install the [SonarScanner CLI](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/).
2. Run the scanner from your project directory:
```bash
sonar-scanner \
  -Dsonar.projectKey=your_project_key \
  -Dsonar.sources=src/main/java \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=your_sonar_token
```

---

### **6. View Results**
Once the scan completes:
1. Open your SonarQube dashboard: [http://localhost:9000](http://localhost:9000).
2. Find your project under the "Projects" tab.
3. Review code quality metrics, bugs, vulnerabilities, and more.

---

The issue lies in your script at this line:

```bash
JAVA_17_HOME="/Library/Java/JavaVirtualMachines/temurin-17.jdk/Contents/Home/bin/java"
```

The `JAVA_HOME` variable should point to the base directory of the JDK, not to the `java` binary itself. Update your script as follows:

---

### **Corrected Script**

```bash
#!/bin/bash

# Paths to Java installations
JAVA_17_HOME="/Library/Java/JavaVirtualMachines/temurin-17.jdk/Contents/Home"
JAVA_22_HOME="/Library/Java/JavaVirtualMachines/jdk-22.jdk/Contents/Home"

# Path to SonarQube
SONARQUBE_HOME="/usr/local/sonarqube"

# Check if Java 17 is installed
if [ ! -d "$JAVA_17_HOME" ]; then
  echo "Java 17 is not installed at $JAVA_17_HOME. Please install Java 17 first."
  exit 1
fi

# Check if SonarQube directory exists
if [ ! -d "$SONARQUBE_HOME" ]; then
  echo "SonarQube directory not found at $SONARQUBE_HOME. Please provide the correct path."
  exit 1
fi

# Backup the current JAVA_HOME
CURRENT_JAVA_HOME=$JAVA_HOME

echo "Switching to Java 17 for SonarQube..."
export JAVA_HOME="$JAVA_17_HOME"

# Confirm the switch
echo "Using Java version:"
$JAVA_HOME/bin/java -version

# Start SonarQube
echo "Starting SonarQube..."
$SONARQUBE_HOME/bin/macosx-universal-64/sonar.sh start

# Wait for user input to stop SonarQube
read -p "Press [Enter] to stop SonarQube and switch back to Java 22..."

# Stop SonarQube
echo "Stopping SonarQube..."
$SONARQUBE_HOME/bin/macosx-universal-64/sonar.sh stop

# Revert to original JAVA_HOME
echo "Switching back to Java 22..."
export JAVA_HOME="$JAVA_22_HOME"

# Confirm the revert
echo "Using Java version:"
$JAVA_HOME/bin/java -version

echo "Done. System is back to Java 22."
```

---

### **Key Fixes**
1. Changed `JAVA_17_HOME` to point to the JDK base directory:
   ```bash
   JAVA_17_HOME="/Library/Java/JavaVirtualMachines/temurin-17.jdk/Contents/Home"
   ```
2. Used `$JAVA_HOME/bin/java` to invoke the Java binary in the `java -version` commands.
3. The script now correctly checks for the existence of the `JAVA_17_HOME` directory.

---

### **Verify**
1. Save the corrected script.
2. Make sure the `JAVA_17_HOME` path is correct (it should match your installed Java 17 directory).
3. Run the script again:
   ```bash
   ./run-sonarqube.sh
   ```

This should resolve the issue! Let me know if you face further problems.
