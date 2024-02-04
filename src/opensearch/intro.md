# OpenSearch

ps:cwa

## Documentação oficial 

[official docs](https://opensearch.org/docs/latest/)


## Instalação Opensearch no Ubuntu 22.04

[como instalar](https://opensearch.org/docs/latest/install-and-configure/install-opensearch/index/)

    #COMANDOS GUIA DE INSTALAÇÃO
    
    cat /proc/sys/vm/max_map_count
    sudo nano /etc/sysctl.conf
    Deixando comentario no final vm.max_map_count=262144
    sudo sysctl -p
    Dando comando encima voltara esse valor vm.max_map_count=262144 
    wget -c https://artifacts.opensearch.org/releases/bundle/opensearch/2.11.0/opensearch-2.11.0-linux-x64.deb
    sudo dpkg -i opensearch-2.6.0-linux-x64.deb
    sudo systemctl daemon-reload
    sudo systemctl enable opensearch
    Inicie o serviço OpenSearch.
    sudo systemctl start opensearch
    Verifique se o OpenSearch foi iniciado corretamente.
    sudo systemctl status opensearch
    curl -XGET https://localhost:9200 -u 'admin:admin' --insecure

## Criar Index   

Execute o comando abaixo

    curl -X PUT https://localhsot:9200/mycolletcion/_doc/1 \
    -H 'Content-Type: application/json' \
    -d '{
    "mappings": {
        "properties": {
            "manchete": { "type": "text" },
            "subtitulo": { "type": "text" },
            "txt": { "type": "text" },
            "resumo": { "type": "text" },
            "dt": { "type": "date", "format": "yyyy-MM-dd" },
            "autor": { "type": "keyword" },
            "veiculo": { "type": "flattened" },
            "monitoramentos": { "type": "flattened" }
        }
    }
    }' \
    -u 'user:password' --insecure

Com isso, o index ja estara Criado       
    








