## Worker1:
- Configure Nginx and Fail2Ban. Follow [V2 Nginx Fail2Ban ReadMe.md](https://github.com/TheSpiritMan/V2-Nginx-Fail2Ban/blob/main/ReadMe.md) file.

### Configure Filebeat:
- Install Filebeat:
    ```sh
    cd /tmp
    curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-9.1.5-amd64.deb
    sudo dpkg -i filebeat-9.1.5-amd64.deb
    ```

- Backup default filebeat config:
    ```sh
    sudo mv /etc/filebeat/filebeat.yml /etc/filebeat/filebeat.yml.bak
    ```

- Copy [filebeat.yml](./filebeat.yml) and paste in `/etc/filebeat/filebeat.yml`.

> [!NOTE]: Remember to change IP Address of Logstash Host in above `filebeat.yml` file.

- Enable Filebeat Modules:
    ```sh
    sudo filebeat modules enable system
    ```

- Test configuration before starting `filebeat` service:
    ```sh
    sudo filebeat test config
    sudo filebeat test output
    ```
    
    <details>
    <summary>Detailed Output</summary>
    <blockquote> 

    ~~~sh
    Config OK
    logstash: 192.168.121.1:5044...
    connection...
        parse host... OK
        dns lookup... OK
        addresses: 192.168.121.1
        dial up... OK
    TLS... WARN secure connection disabled
    talk to server... OK
    ~~~

    </blockquote>
    </details>

- Start systemctl service:
    ```sh
    sudo systemctl stop filebeat
    sudo systemctl disable filebeat
    sudo systemctl enable filebeat
    sudo systemctl start filebeat
    sudo systemctl status filebeat
    ```

### Configure Metricbeat
- Install Metricbeat:
    ```sh
    cd /tmp
    curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-9.1.5-amd64.deb
    sudo dpkg -i metricbeat-9.1.5-amd64.deb
    ```

- Backup default metricbeat config:
    ```sh
    sudo mv /etc/metricbeat/metricbeat.yml /etc/metricbeat/metricbeat.yml.bak
    ```

- Copy [metricbeat.yml](./metricbeat.yml) and paste in `/etc/metricbeat/metricbeat.yml`.

> [!NOTE]: Remember to change IP Address of Logstash Host in above `metricbeat.yml` file.

- Enable Metricbeat Modules:
    ```sh
    sudo metricbeat modules enable system
    ```

- Test configuration before starting `metricbeat` service:
    ```sh
    sudo filebeat test config
    sudo filebeat test output
    ```
    
    <details>
    <summary>Detailed Output</summary>
    <blockquote> 

    ~~~sh
    Config OK
    logstash: 192.168.121.1:5044...
    connection...
        parse host... OK
        dns lookup... OK
        addresses: 192.168.121.1
        dial up... OK
    TLS... WARN secure connection disabled
    talk to server... OK
    ~~~

    </blockquote>
    </details>

- Start systemctl service:
    ```sh
    sudo systemctl disable metricbeat
    sudo systemctl stop metricbeat
    sudo systemctl enable metricbeat
    sudo systemctl start metricbeat
    sudo systemctl status metricbeat
    ```

### Remove deb package:
- Command:
    ```sh
    sudo rm /tmp/metricbeat-9.1.5-amd64.deb /tmp/filebeat-9.1.5-amd64.deb
    ```


## Debugging
- Test for Nginx Log:
    ```sh
    curl -k -u elastic:M7u3Y16wm9W06IoYw08n1qok \
    -H "Content-Type: application/json" \
    https://localhost:9200/beats-*/_search?pretty \
    -d '{
        "query": { "match": { "tags": "nginx" } }
    }'
    ```

- Test for Fail2Ban Log:

    ```sh
    curl -k -u elastic:M7u3Y16wm9W06IoYw08n1qok \
    -H "Content-Type: application/json" \
    https://localhost:9200/beats-*/_search?pretty \
    -d '{
        "query": { "match": { "tags": "fail2ban" } }
    }'
    ```

- `Worker1` Host within 1 hour:
    ```sh
    curl -k -u elastic:M7u3Y16wm9W06IoYw08n1qok -H "Content-Type: application/json" https://localhost:9200/beats-*/_search?pretty -d '{
    "query": {
        "bool": {
        "must": [
            { "match": { "host.name": "worker1" } },
            { "range": { "@timestamp": { "gte": "now-5m" } } }
        ]
        }
    }
    }'
    ```