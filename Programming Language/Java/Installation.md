# Ubuntu

## Download OpenJDK

```
sudo apt update
sudo apt install default-jdk
```

## Set default Java if have multiple Java installations

```
sudo update-alternatives --config java
```

## Set JAVA_HOME

```
sudo update-alternatives --config java
```

OpenJDK 11 is locaed at `/usr/lib/jvm/java-11-openjdk-amd64/bin/java`

```
sudo nano /etc/environment
```

At the end of the file, add the following line, <b>DO NOT</b> include the `/bin` portion of the path:

```
JAVA_HOME="usr/lib/jvm/java-11-openjdk-amd64"
```

Save the file and exit the editor. Reload the file to apply `JAVA_HOME`

```
source /etc/environment
```

Confirm.

```
java -version
echo $JAVA_HOME
```
