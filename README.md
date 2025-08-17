Sure! Here's a clean, ready-to-use **README.md** you can put in your project root. It documents all the steps you did to build and automate your Java Maven project with Jenkins + Docker + GitHub webhook + ngrok:

---

```markdown
# Java Maven Jenkins Build Automation - Task 8

This project demonstrates how to automate building a simple Java Maven project using Jenkins running in Docker, triggered by GitHub webhooks via ngrok tunneling.

---

## Project Structure

```

Java-mavenDevopsTask8/
├── pom.xml
└── src/
└── main/
└── java/
└── HelloWorld.java

````

---

## Step 1: Java Application & Maven Setup

- Created a simple Java program `HelloWorld.java` printing a message.
- Created a `pom.xml` Maven configuration file with Java 1.8 compiler settings.

---

## Step 2: Run Jenkins in Docker

```bash
docker run -p 8080:8080 -p 50000:50000 --name jenkins-lts -d jenkins/jenkins:lts
````

* Access Jenkins UI: [http://localhost:8080](http://localhost:8080)
* Retrieve initial admin password:

```bash
docker exec jenkins-lts cat /var/jenkins_home/secrets/initialAdminPassword
```

* Installed suggested plugins and created admin user.

---

## Step 3: Configure Jenkins Tools

* Installed Maven via **Manage Jenkins > Global Tool Configuration**
* Added Maven installation named "Maven 3.8.6" with auto-install.

---

## Step 4: Create Jenkins Job

* Created a Freestyle project named `HelloJavaMaven`.
* Under **Build** step, added **Invoke top-level Maven targets** with goals:

```
clean package
```

* Configured GitHub repository URL under **Source Code Management**.
* Enabled **Build Triggers** → **GitHub hook trigger for GITScm polling**.

---

## Step 5: Setup ngrok for GitHub Webhook

* Started ngrok to expose Jenkins:

```bash
ngrok http 8080
```

* Used the generated forwarding URL (e.g. `https://dbf0628bd7ec.ngrok-free.app`) to configure GitHub webhook:

Webhook URL:

```
https://dbf0628bd7ec.ngrok-free.app/github-webhook/
```

---

## Step 6: Push code & Trigger build

* Made a code change:

```bash
echo "// Trigger webhook test" >> src/main/java/HelloWorld.java
git add src/main/java/HelloWorld.java
git commit -m "Trigger Jenkins build via GitHub webhook"
git push
```

* Jenkins receives webhook and automatically starts the build.
* Build status and console output available at:

```
http://localhost:8080/job/HelloJavaMaven/
```

---

## Notes

* Make sure the GitHub webhook URL is updated whenever ngrok restarts (unless you have a paid ngrok plan with reserved URLs).
* Verify Jenkins has permission to access the GitHub repo.
* Check Jenkins logs and ngrok web interface at [http://127.0.0.1:4040](http://127.0.0.1:4040) for troubleshooting.

---

## Summary Commands

```bash
mkdir -p src/main/java
cat > src/main/java/HelloWorld.java <<EOF
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, Jenkins + Maven!");
    }
}
EOF

cat > pom.xml <<EOF
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>hello</artifactId>
    <version>1.0</version>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
EOF

docker run -p 8080:8080 -p 50000:50000 --name jenkins-lts -d jenkins/jenkins:lts
```

---

## Contact / Help

Feel free to reach out for any troubleshooting or help completing this task!

---

*End of README.md*

```

---

If you want, I can also help generate the exact file so you just save and commit it! Would you like me to do that?
```
