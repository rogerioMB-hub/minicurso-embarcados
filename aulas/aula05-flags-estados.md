---
layout: default
title: "Aula 5 — Flags e Máquina de Estados em um Byte"
---

# Aula 5 — Flags e Máquina de Estados em um Byte

> **Duração estimada:** 30 minutos  
> **Bloco:** 3 de 3 — Integração e preparação para UART

---

## Objetivos

Ao final desta aula você será capaz de:

- Usar um único byte como conjunto de flags independentes
- Setar, limpar e testar flags com `|=`, `&= ~`, `^=` e `&`
- Implementar uma lógica de estados representada por bits
- Refletir o estado do sistema nos LEDs em tempo real

---

## 1. Conceito

### O byte como conjunto de flags

Uma **flag** é um bit usado para sinalizar que uma condição está ativa (`1`) ou inativa (`0`). Um byte de 8 bits pode carregar **8 flags independentes** ao mesmo tempo.

```
Byte de estado:   0b 0 0 0 0 F3 F2 F1 F0
                              │  │  │  └── Flag 0: sistema ligado
                              │  │  └───── Flag 1: modo manual
                              │  └──────── Flag 2: alarme ativo
                              └─────────── Flag 3: erro crítico
```

### Operações sobre flags

| Operação | Código | Efeito |
|----------|--------|--------|
| Setar flag N | `estado \|= (1 << N)` | Coloca bit N em `1`, preserva os demais |
| Limpar flag N | `estado &= ~(1 << N)` | Coloca bit N em `0`, preserva os demais |
| Alternar flag N | `estado ^= (1 << N)` | Inverte bit N (toggle) |
| Testar flag N | `estado & (1 << N)` | Retorna valor não-nulo se bit N for `1` |

> **Por que `&= ~(1 << N)` limpa o bit?**  
> `~(1 << N)` gera uma máscara com todos os bits em `1` exceto o bit N.  
> O AND preserva todos os bits que estão em `1` na máscara — e zera apenas o bit N.

---

## 2. Circuito

| Componente | Quantidade |
|------------|------------|
| ESP32 DevKit | 1 |
| LED | 4 |
| Resistor 220 Ω | 4 |
| Botão (pushbutton) | 2 |
| Resistor 10 kΩ | 2 |

**Conexões:**

```
GPIO2  ──► R220 ──► LED0 ──► GND   (Flag 0)
GPIO4  ──► R220 ──► LED1 ──► GND   (Flag 1)
GPIO5  ──► R220 ──► LED2 ──► GND   (Flag 2)
GPIO18 ──► R220 ──► LED3 ──► GND   (Flag 3)

GPIO13 ──► Botão A ──► 3,3V | GPIO13 ──► 10kΩ ──► GND
GPIO14 ──► Botão B ──► 3,3V | GPIO14 ──► 10kΩ ──► GND
```

```
# Pico: LEDs em GPIO 0,1,2,3 | Botões em GPIO 14,15
```

---

## 3. Código

```python
from machine import Pin
import time

# --- pinos ---
leds = [Pin(2,Pin.OUT), Pin(4,Pin.OUT), Pin(5,Pin.OUT), Pin(18,Pin.OUT)]
btn_a = Pin(13, Pin.IN)   # seta / alterna flags
btn_b = Pin(14, Pin.IN)   # limpa flags
# Pico: btn_a = Pin(14, Pin.IN) | btn_b = Pin(15, Pin.IN)

# --- definição das flags como constantes ---
FLAG_LIGADO  = 0b00000001   # bit 0
FLAG_MANUAL  = 0b00000010   # bit 1
FLAG_ALARME  = 0b00000100   # bit 2
FLAG_ERRO    = 0b00001000   # bit 3

estado = 0b00000000   # início: tudo inativo

def aplicar_estado(byte_estado):
    """Escreve os 4 bits menos significativos nos 4 LEDs."""
    for i, led in enumerate(leds):
        led.value((byte_estado >> i) & 1)

def print_estado(byte_estado):
    print(f"estado = 0b{byte_estado:08b}")
    print(f"  LIGADO={bool(byte_estado & FLAG_LIGADO)}  "
          f"MANUAL={bool(byte_estado & FLAG_MANUAL)}  "
          f"ALARME={bool(byte_estado & FLAG_ALARME)}  "
          f"ERRO={bool(byte_estado & FLAG_ERRO)}")

# --- sequência de demonstração ---
print("=== Demonstração de flags ===")

# seta FLAG_LIGADO
estado |= FLAG_LIGADO
print("Setou FLAG_LIGADO:"); print_estado(estado); aplicar_estado(estado); time.sleep(1)

# seta FLAG_MANUAL
estado |= FLAG_MANUAL
print("Setou FLAG_MANUAL:"); print_estado(estado); aplicar_estado(estado); time.sleep(1)

# seta FLAG_ALARME
estado |= FLAG_ALARME
print("Setou FLAG_ALARME:"); print_estado(estado); aplicar_estado(estado); time.sleep(1)

# limpa FLAG_MANUAL
estado &= ~FLAG_MANUAL
print("Limpou FLAG_MANUAL:"); print_estado(estado); aplicar_estado(estado); time.sleep(1)

# alterna FLAG_ALARME (toggle)
estado ^= FLAG_ALARME
print("Toggle FLAG_ALARME:"); print_estado(estado); aplicar_estado(estado); time.sleep(1)

# --- loop interativo com botões ---
print("\n=== Modo interativo ===")
print("Botão A: alterna FLAG_MANUAL e FLAG_ALARME")
print("Botão B: limpa todas as flags (reset)")

estado = FLAG_LIGADO   # sistema sempre ligado

while True:
    if btn_a.value():
        estado ^= FLAG_MANUAL   # toggle manual
        estado ^= FLAG_ALARME   # toggle alarme
        aplicar_estado(estado)
        print_estado(estado)
        time.sleep(0.3)         # debounce simples

    if btn_b.value():
        estado = FLAG_LIGADO    # reset — mantém apenas FLAG_LIGADO
        aplicar_estado(estado)
        print("Reset!"); print_estado(estado)
        time.sleep(0.3)

    time.sleep(0.05)
```

---

## 4. Experimento

Execute o código e observe a sequência de demonstração.

**a)** Complete a tabela após cada operação da demonstração:

| Operação | `estado` em binário | LEDs acesos |
|----------|:-------------------:|:-----------:|
| início | `0b00000000` | nenhum |
| `\|= FLAG_LIGADO` | `0b________` | LED____ |
| `\|= FLAG_MANUAL` | `0b________` | LED____ |
| `\|= FLAG_ALARME` | `0b________` | LED____ |
| `&= ~FLAG_MANUAL` | `0b________` | LED____ |
| `^= FLAG_ALARME` | `0b________` | LED____ |

**b)** Por que usar `estado |= FLAG_MANUAL` é melhor do que `estado = estado + 2`?

> _________________________________________________________________

**c)** A expressão `~FLAG_MANUAL` em Python, quando `FLAG_MANUAL = 0b00000010`:
- Qual o resultado? `_____`
- Por que ainda funciona corretamente no `&=`? (dica: o AND com bits extras não muda nada)

> _________________________________________________________________

**d)** No modo interativo, pressione o Botão A duas vezes seguidas. O que aconteceu com FLAG_ALARME? Por que?

> _________________________________________________________________

---

## 5. Desafio

**Desafio principal:** adicione uma flag `FLAG_ERRO` (bit 3) com a seguinte regra:

> Quando `FLAG_ERRO` for setada, todos os outros LEDs devem apagar — só o LED3 fica aceso, independente do valor das demais flags.

Modifique a função `aplicar_estado` para implementar essa prioridade:

```python
def aplicar_estado(byte_estado):
    if byte_estado & FLAG_ERRO:
        # erro crítico: apaga tudo e acende só LED3
        for i, led in enumerate(leds):
            led.value(__________)
    else:
        # comportamento normal
        for i, led in enumerate(leds):
            led.value((byte_estado >> i) & 1)
```

**Desafio bônus:** crie uma função `flags_ativas(estado)` que retorne uma lista com os nomes das flags que estão em `1`:

```python
def flags_ativas(estado):
    nomes = {FLAG_LIGADO: "LIGADO", FLAG_MANUAL: "MANUAL",
             FLAG_ALARME: "ALARME", FLAG_ERRO: "ERRO"}
    return [nome for flag, nome in nomes.items() if estado & flag]
```

Teste e verifique o retorno para diferentes valores de `estado`.

---

## Resumo da aula

- Um byte pode carregar 8 flags independentes — cada bit representa uma condição
- `|= (1 << N)` seta o bit N sem afetar os demais
- `&= ~(1 << N)` limpa o bit N sem afetar os demais
- `^= (1 << N)` alterna o bit N (toggle)
- `& mascara` testa se um bit está setado
- Flags tornam o código legível e o estado compacto — exatamente como bytes de status em protocolos de comunicação

---

*← [Aula 4](./aula04-deslocamento-escrita-porta.md) | Próxima → [Aula 6: Bytes pela UART](./aula06-uart-bytes.md)*
