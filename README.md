
# Apache Druid Kafka and ADLS source integration
1. `docker-compose up`
2. Ingest some data from the ADLS to Druid
3. Get inside the kafka container: `docker exec -it druid_kafka_1 /bin/bash`
4. Start kafka producer and publish some continuously data: 
`for x in {221..500}; do echo "{employee_id:$x,department_id:$(( ( RANDOM % 5 )  + 1 ))}"; sleep 2; done | kafka-console-producer \
--topic employee-enter-logs --broker-list localhost:9092`
- `{employee_id:1,department_id:1}`
- `{employee_id:2,department_id:5}`
- `...`
5. Set up kafka connection through Druid UI and define your topic as druid table.
6. Let's enrich the streaming data:
     `SELECT
          e."__time" employee_enter_time,
          e."department_id",
          e."employee_id",
          d."department_name"
        FROM "employee-enter-logs" e JOIN "publicdepartments" d
        ON e."department_id"=d."department_id"`
