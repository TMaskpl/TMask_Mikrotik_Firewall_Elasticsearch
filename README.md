# TMask_Mikrotik_Firewall_Elasticsearch

========================


Przekierowujemy logi na Logstash port 5044/udp


[code]

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

[/code]
