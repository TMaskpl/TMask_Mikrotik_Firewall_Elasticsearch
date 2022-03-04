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

![2022-03-04_12-57](https://user-images.githubusercontent.com/75216446/156763805-abd3b9c3-197e-43c6-8d6c-628d277608f9.png)




:)


![2022-03-04_13-25](https://user-images.githubusercontent.com/75216446/156763828-fce40b94-578c-4181-9962-924d6ea9e5c5.png)

