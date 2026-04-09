---
layout: default
title: "Referências e Links Wokwi"
---

# Referências e Links para o Wokwi

## Como criar um projeto no Wokwi

1. Acesse [wokwi.com](https://wokwi.com)
2. Clique em **New Project**
3. Selecione **ESP32** → **MicroPython**
4. Monte o circuito na aba **Diagram**
5. Cole o código na aba **main.py**
6. Clique em **Play** para simular

---

## Templates de circuito por aula

Os templates abaixo podem ser colados diretamente no arquivo `diagram.json` do Wokwi.

### Aulas 1 e 2 — 1 LED + 1 ou 2 botões

```json
{
  "version": 1,
  "author": "Mini Curso Embarcados",
  "editor": "wokwi",
  "parts": [
    { "type": "wokwi-esp32-devkit-v1", "id": "esp", "top": 0, "left": 0, "attrs": {} },
    { "type": "wokwi-led", "id": "led1", "top": 100, "left": 200, "attrs": { "color": "red" } },
    { "type": "wokwi-resistor", "id": "r1", "top": 140, "left": 200, "attrs": { "value": "220" } },
    { "type": "wokwi-pushbutton", "id": "btn1", "top": 100, "left": 350, "attrs": {} },
    { "type": "wokwi-resistor", "id": "r2", "top": 140, "left": 350, "attrs": { "value": "10000" } }
  ],
  "connections": [
    ["esp:2", "r1:1", "green", []],
    ["r1:2", "led1:A", "green", []],
    ["led1:K", "esp:GND.1", "black", []],
    ["esp:4", "btn1:1.l", "blue", []],
    ["btn1:2.l", "esp:3V3", "red", []],
    ["btn1:1.l", "r2:1", "blue", []],
    ["r2:2", "esp:GND.1", "black", []]
  ]
}
```

### Aulas 3, 4 e 5 — 4 LEDs + 2 botões

```json
{
  "version": 1,
  "author": "Mini Curso Embarcados",
  "editor": "wokwi",
  "parts": [
    { "type": "wokwi-esp32-devkit-v1", "id": "esp", "top": 0, "left": 0, "attrs": {} },
    { "type": "wokwi-led", "id": "led0", "top": 80,  "left": 250, "attrs": { "color": "red"    } },
    { "type": "wokwi-led", "id": "led1", "top": 80,  "left": 290, "attrs": { "color": "yellow" } },
    { "type": "wokwi-led", "id": "led2", "top": 80,  "left": 330, "attrs": { "color": "green"  } },
    { "type": "wokwi-led", "id": "led3", "top": 80,  "left": 370, "attrs": { "color": "blue"   } },
    { "type": "wokwi-resistor", "id": "r0", "top": 120, "left": 250, "attrs": { "value": "220" } },
    { "type": "wokwi-resistor", "id": "r1", "top": 120, "left": 290, "attrs": { "value": "220" } },
    { "type": "wokwi-resistor", "id": "r2", "top": 120, "left": 330, "attrs": { "value": "220" } },
    { "type": "wokwi-resistor", "id": "r3", "top": 120, "left": 370, "attrs": { "value": "220" } },
    { "type": "wokwi-pushbutton", "id": "btnA", "top": 180, "left": 250, "attrs": {} },
    { "type": "wokwi-pushbutton", "id": "btnB", "top": 180, "left": 310, "attrs": {} },
    { "type": "wokwi-resistor", "id": "rA", "top": 220, "left": 250, "attrs": { "value": "10000" } },
    { "type": "wokwi-resistor", "id": "rB", "top": 220, "left": 310, "attrs": { "value": "10000" } }
  ],
  "connections": [
    ["esp:2",  "r0:1", "green", []], ["r0:2", "led0:A", "green", []], ["led0:K", "esp:GND.1", "black", []],
    ["esp:4",  "r1:1", "green", []], ["r1:2", "led1:A", "green", []], ["led1:K", "esp:GND.1", "black", []],
    ["esp:5",  "r2:1", "green", []], ["r2:2", "led2:A", "green", []], ["led2:K", "esp:GND.1", "black", []],
    ["esp:18", "r3:1", "green", []], ["r3:2", "led3:A", "green", []], ["led3:K", "esp:GND.1", "black", []],
    ["esp:13", "btnA:1.l", "blue", []], ["btnA:2.l", "esp:3V3", "red", []], ["btnA:1.l", "rA:1", "blue", []], ["rA:2", "esp:GND.1", "black", []],
    ["esp:14", "btnB:1.l", "blue", []], ["btnB:2.l", "esp:3V3", "red", []], ["btnB:1.l", "rB:1", "blue", []], ["rB:2", "esp:GND.1", "black", []]
  ]
}
```

---

## Mapeamento de pinos

### ESP32 (principal)

| Função | GPIO | Aulas |
|--------|------|-------|
| LED0 (bit 0) | 2 | 1–6 |
| LED1 (bit 1) | 4 | 1–6 |
| LED2 (bit 2) | 5 | 3–6 |
| LED3 (bit 3) | 18 | 3–6 |
| Botão A | 4 (aula 2), 13 (aulas 5+) | 2, 5 |
| Botão B | 5 (aula 2), 14 (aulas 5+) | 2, 5 |
| UART TX | 1 | 6 |
| UART RX | 3 | 6 |

### Raspberry Pi Pico (alternativa)

| Função | GPIO |
|--------|------|
| LED0 | 0 |
| LED1 | 1 |
| LED2 | 2 |
| LED3 | 3 |
| Botão A | 14 |
| Botão B | 15 |
| UART0 TX | 0 |
| UART0 RX | 1 |

> Para o Pico, selecione **Raspberry Pi Pico** ao criar o projeto no Wokwi.  
> O código MicroPython é idêntico — ajuste apenas os números dos pinos conforme a tabela acima.

---

## Referências

- [Documentação MicroPython — machine.Pin](https://docs.micropython.org/en/latest/library/machine.Pin.html)
- [Documentação MicroPython — machine.UART](https://docs.micropython.org/en/latest/library/machine.UART.html)
- [ESP32 Technical Reference Manual — GPIO](https://www.espressif.com/sites/default/files/documentation/esp32_technical_reference_manual_en.pdf)
- [Wokwi ESP32 + MicroPython](https://docs.wokwi.com/pt-BR/guides/micropython)
- [Wokwi Raspberry Pi Pico](https://docs.wokwi.com/pt-BR/parts/wokwi-pi-pico)
