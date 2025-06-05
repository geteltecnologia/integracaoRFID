# Guia de Integração: Conexão TCP/IP e Leitura de Tags RFID (Chainway UR4) com Informação da Antena

---

## 1. Dependências

Adicione a biblioteca do leitor ao projeto e importe as classes necessárias:

```java
import com.example.uhf.DeviceAPI.IUHFURx;
import com.example.uhf.DeviceAPI.RFIDWithUHFUrxNetwork;
import com.example.uhf.DeviceAPI.OnSpdInventoryListener;
import com.example.uhf.DeviceAPI.SpdInventoryData;
```

---

## 2. Fluxo de Implementação

### a) Instanciar o leitor TCP/IP

```java
IUHFURx mReader = RFIDWithUHFUrxNetwork.getInstance();
```

---

### b) Conectar ao leitor via TCP/IP

```java
boolean conectado = mReader.init("IP_DO_LEITOR", PORTA_DO_LEITOR);
// Exemplo: mReader.init("192.168.1.100", 6000);
```

---

### c) Configurar o listener para receber as tags lidas (incluindo antena)

```java
mReader.setOnInventoryListener(new OnSpdInventoryListener() {
    @Override
    public void getInventoryData(SpdInventoryData data) {
        String epc     = data.getEpc();
        int rssi       = data.getRssi();
        int count      = data.getCount();
        int antenna    = data.getAntenna(); // Informação da antena

        // Exemplo de uso:
        System.out.println(
            "EPC: "     + epc    +
            " | RSSI: " + rssi   +
            " | Count: "+ count  +
            " | Antena: "+ antenna
        );

        // Faça o processamento necessário (exibir, salvar, etc)
    }
});
```

---

### d) Iniciar a leitura de tags

```java
mReader.startInventoryTag();
```

---

### e) Parar a leitura (quando necessário)

```java
mReader.stopInventory();
```

---

### f) Liberar recursos ao finalizar

```java
mReader.free();
```

---

## 3. Exemplo Resumido

```java
IUHFURx mReader = RFIDWithUHFUrxNetwork.getInstance();
boolean conectado = mReader.init("192.168.1.100", 6000);

if (conectado) {
    mReader.setOnInventoryListener(new OnSpdInventoryListener() {
        @Override
        public void getInventoryData(SpdInventoryData data) {
            String epc     = data.getEpc();
            int rssi       = data.getRssi();
            int count      = data.getCount();
            int antenna    = data.getAntenna();

            System.out.println(
                "EPC: "     + epc    +
                " | RSSI: " + rssi   +
                " | Count: "+ count  +
                " | Antena: "+ antenna
            );
        }
    });

    mReader.startInventoryTag();
}
```
