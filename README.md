# Kafka UI

Web-based administration for your Kafka broker at **kafka.alpha5.finance:9092**, powered by [Kafbat UI](https://github.com/kafbat/kafka-ui).

## Single node is fine

Kafka calls any set of brokers a **cluster**, even when there is only **one** broker. This UI is not limited to multi-node setups.

Your server at `kafka.alpha5.finance:9092` is a valid one-broker cluster. The UI will connect to that single bootstrap address and show **1 broker**, your topics, consumer groups, and messages — the same as a larger deployment, just with one node.

## Quick start

1. Copy the env file and point it at your broker:

   ```bash
   cp .env.example .env
   ```

2. Broker address is already set in `.env`:

   ```env
   KAFKA_BOOTSTRAP_SERVERS=kafka.alpha5.finance:9092
   ```

3. Start:

   ```bash
   docker compose up -d
   ```

4. Open **http://localhost:8080**

## Where is your Kafka running?

| Setup | `KAFKA_BOOTSTRAP_SERVERS` |
|-------|---------------------------|
| Kafka on this Windows machine | `host.docker.internal:9092` |
| Kafka on another server | `192.168.x.x:9092` or `my-server:9092` |
| Kafka in Docker on the same host | Use that container's published port, e.g. `host.docker.internal:9092` |

Kafka must advertise an address the UI container can reach. If the UI shows connection errors, check `advertised.listeners` on your broker.

## What you can do in the UI

- Topics — create, browse, produce/consume messages
- Consumer groups — lag and offsets
- Brokers — health and configuration
- ACLs — if enabled on your broker

## Local dev (optional)

To run a throwaway single-node Kafka plus the UI for testing:

```bash
docker compose -f docker-compose.dev.yml up -d
```

## SASL / SSL

If your broker uses authentication, edit `config/dynamic_config.yaml`:

```yaml
kafka:
  clusters:
    - name: kafka
      bootstrap-servers: your-host:9092
      properties:
        security.protocol: SASL_SSL
        sasl.mechanism: SCRAM-SHA-512
        sasl.jaas.config: org.apache.kafka.common.security.scram.ScramLoginModule required username="user" password="pass";
```

## Commands

```bash
docker compose up -d
docker compose logs -f kafbat-ui
docker compose down
```
