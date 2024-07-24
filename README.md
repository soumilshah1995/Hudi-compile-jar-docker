![Screenshot 2024-07-24 at 9 55 54 AM](https://github.com/user-attachments/assets/90c20b5c-1903-4c03-87d8-d4893a486cd4)

## Commands
```
# Start docker linux container
docker run -it rockylinux:9

# install packages and java
dnf install -y java-1.8.0-openjdk java-1.8.0-openjdk-devel maven git ncurses

# Clone hudi project
cd /home
git clone https://github.com/apache/hudi.git && cd hudi
git checkout release-1.0.0-beta2

# Set enviroment VAR
export JAVA_HOME=/usr/lib/jvm/java-1.8.0
export PATH=$JAVA_HOME/bin:$PATH

# Build against Spark 3.4.x
mvn clean package -DskipTests -Dspark3.4

```

## mybash.sh
```
#!/bin/bash

# Variables
SPARK_VERSION="3.4"
HUDI_VERSION="1.0.0-beta2"
IMAGE_NAME="rockylinux:9"  # Image name to match

# Get the container ID for the specified image
CONTAINER_ID=$(docker ps -q -f "ancestor=${IMAGE_NAME}")

# Check if the container ID was found
if [ -z "$CONTAINER_ID" ]; then
  echo "No container found with image '${IMAGE_NAME}'."
  exit 1
fi

# Define the paths and filenames with placeholders
UTILITIES_BUNDLE_PATH="/home/hudi/packaging/hudi-utilities-bundle/target/hudi-utilities-bundle_2.12-${HUDI_VERSION}.jar"
AWS_BUNDLE_PATH="/home/hudi/packaging/hudi-aws-bundle/target/hudi-aws-bundle-${HUDI_VERSION}.jar"
SPARK_BUNDLE_PATH="/home/hudi/packaging/hudi-spark-bundle/target/hudi-spark${SPARK_VERSION}-bundle_2.12-${HUDI_VERSION}.jar"

# Target path on the host
TARGET_PATH="."

# Copy files from the container to the host
docker cp "${CONTAINER_ID}:${UTILITIES_BUNDLE_PATH}" "${TARGET_PATH}"
docker cp "${CONTAINER_ID}:${AWS_BUNDLE_PATH}" "${TARGET_PATH}"
docker cp "${CONTAINER_ID}:${SPARK_BUNDLE_PATH}" "${TARGET_PATH}"

echo "Files copied successfully."

```


## References 
* https://medium.com/@life-is-short-so-enjoy-it/hudi-build-your-jars-with-your-patches-rocky-linux-in-docker-9010b275321a
  
