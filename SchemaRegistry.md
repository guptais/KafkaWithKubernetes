#=-=-=-=-=-=-=-=-=-=-=-=-=- Building

# Checkout Confluent Common - Pull a specific build pinned against a Kafka release eg. 5.2.2
git clone --branch 5.2.2-post https://github.com/confluentinc/common
# Build it + install to local m2 cache
mvn clean package install

# Check out Confluent Rest - Pull matching release as Confluent Common
git clone --branch 5.2.2-post https://github.com/confluentinc/rest-utils
# Build it + install to local m2 cache
mvn clean package install

# Check out Confluent Schema Registry - Pull matching release as Confluent Common
git clone --branch 5.2.2-post https://github.com/confluentinc/schema-registry
# Build it
mvn clean package
# Run it
./bin/schema-registry-start config/schema-registry.properties