# TMask_Mikrotik_Firewall_Elasticsearch

========================


Przekierowujemy logi na Logstash port 5044/udp


```
input {
    syslog {
        port => "5044"
    }

}
filter {
    grok {
        match => { "message" => "%{DATA:LogPrefix} %{DATA:LogChain}: in:%{DATA:src_zone} out:%{DATA:dst_zone}, proto %{DATA:proto}, %{IP:src_ip}:%{INT:src_port}->%{IP:dst_ip}:%{INT:dst_port}, len %{INT:length}" }
    }
    geoip {
        source => "src_ip"
    }
}
output {
    elasticsearch {
        hosts => "elasticsearch:9200"
    }
}

```

Następnie w Kibanie mamy piękny dashboard który obrazuje z jakich lokalizację mamu próbę dostępu do routera

![elastic1](https://user-images.githubusercontent.com/75216446/157237161-318efe4a-2f12-4677-881c-2438e0a31096.png)



![elastic2](https://user-images.githubusercontent.com/75216446/157237189-e6559ae4-3b53-4eef-b5b7-33dd9d83efe5.png)




