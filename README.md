#Monitoramento de Temperatura e Umidade
**REST API | Deploy Render**

## 📌 Informações do Projeto

**Autora:** Isabelle dos Santos de Abreu  
**Turma:** DS3B  
**Disciplina:** Internet das Coisas (IoT)

---

## 📖 Descrição

Este projeto é uma evolução da nossa prática em IoT. Enquanto anteriormente exploramos o protocolo MQTT com o Adafruit IO, nesta etapa focamos na integração de hardware com **APIs REST**.

O sistema utiliza um microcontrolador **ESP32** para coletar dados de temperatura e umidade através do sensor **DHT11**. Diferente do modelo anterior, os dados agora são enviados via requisições **HTTP GET** para uma API hospedada no **Render**, permitindo uma arquitetura mais flexível e integrada a sistemas web modernos.

---

## 🎯 Objetivos

- Compreender a comunicação entre dispositivos embarcados e serviços web (REST API).
- Implementar requisições HTTP utilizando a biblioteca `HTTPClient` no ambiente Arduino.
- Realizar o deploy de um servidor de backend na plataforma Render.
- Aplicar lógica de controle para otimização de tráfego (evitar envios repetidos de dados que não sofreram alteração).

---

## 🛠️ Tecnologias e Componentes Utilizados

### Hardware
- ESP32
- Sensor DHT11
- Protoboard e Jumpers

### Software e Serviços
- **Arduino IDE** (Linguagem C++)
- **Render** (Hospedagem da API)
- **HTTP/REST** (Protocolo de comunicação)

---
## 🔗 Configuração do Servidor

A API está hospedada no Render e processa os dados enviados pelo ESP32 através do seguinte endpoint:

**URL Base:** `https://api-dht11.onrender.com`

### Endpoint de Registro
`GET /sensor`

**Exemplo de Link para Teste:**
[https://api-dht11.onrender.com/sensor?temp=25&hum=60](https://api-dht11.onrender.com/sensor?temp=25&hum=60)

---
## ⚙️ Funcionamento da Lógica

O código foi estruturado para garantir eficiência no consumo de recursos da rede:

1. **Leitura:** O sensor DHT11 captura os valores de temperatura e umidade.
2. **Otimização:** Foi implementada uma verificação de estado (`ultimaTemp` e `ultimaUmidade`) para evitar requisições desnecessárias caso os valores não tenham sofrido alterações significativas.
3. **Comunicação:** O ESP32 realiza uma requisição HTTP GET para o servidor, passando os dados via parâmetros de URL.

```cpp
// Exemplo da montagem da URL de envio:
String url = serverName + "?temp=" + String(temp) + "&hum=" + String(hum);
http.begin(url);
int httpResponseCode = http.GET();

