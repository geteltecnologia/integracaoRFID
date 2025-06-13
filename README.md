# Guia de Integração: Conexão UART e Leitura de Tags RFID com Informação da Antena

---

## 1. Dependências

Adicione a biblioteca do leitor ao projeto e importe as classes necessárias:

```java
import com.rscja.deviceapi.RFIDWithUHFUrxUart;
import com.rscja.deviceapi.interfaces.IUHFURx;
```

---

## 2. Fluxo de Implementação

### a) Instanciar o leitor TCP/IP

```java
IUHFURx mReader = RFIDWithUHFUrxNetwork.getInstance();
```

---

### b) Conectar ao leitor via UART

```java
mReader = RFIDWithUHFUrxUsbToUart.getInstance();
((RFIDWithUHFUrxUsbToUart) mReader).setUart(uartPath);
isConnected = mReader.init(this);
```

---

### c) Configurar o listener para receber as tags lidas (incluindo antena)

```java
mReader.setInventoryCallback(new IUHFInventoryCallback() {
                @Override
                public void callback(UHFTAGInfo tag) {
                    runOnUiThread(() -> {
                        tvResult.append("EPC: " + tag.getEPC() +
                                        " | RSSI: " + tag.getRssi() +
                                        " | Antena: " + tag.getAntenna() + "\n");
                    });
                }
            });
mReader.stopInventory();
```
---

### c) Finalizar conexão do UR4

```java
mReader.free();
```
