---
layout: default
title: "Aula 2 — Seu Primeiro Operador Bitwise em Hardware"
---

# Aula 2 — Seu Primeiro Operador Bitwise em Hardware

> **Duração estimada:** 30 minutos  
> **Bloco:** 1 de 3 — Fundamentos digitais

---

## Objetivos

Ao final desta aula você será capaz de:

- Aplicar os operadores `&` (AND), `|` (OR) e `~` (NOT) sobre valores de pinos reais
- Preencher uma tabela-verdade a partir do comportamento observado no hardware
- Construir lógica combinatória sem usar `if`
- Relacionar cada operador bitwise com a porta lógica equivalente

---

## 1. Conceito

Na aula anterior trabalhamos com um único bit. Agora vamos **operar sobre bits** — exatamente o que as portas lógicas fazem no hardware.

Os três operadores fundamentais em Python:

| Operador | Símbolo | Porta lógica equivalente | Exemplo |
|----------|---------|--------------------------|---------|
| AND      | `&`     | AND (∧)                  | `1 & 0 = 0` |
| OR       | `\|`    | OR (∨)                   | `1 \| 0 = 1` |
| NOT      | `~`     | NOT (¬)                  | `~0 = -1` ⚠️ |

> **Atenção com o NOT (`~`) em Python:** `~0` retorna `-1`, não `1`, porque Python usa
> inteiros com sinal. Para inverter um bit de forma segura, use `valor ^ 1` ou `(~valor) & 1`.
> Veremos isso no experimento.

---

## 2. Circuito

| Componente | Quantidade |
|------------|------------|
| ESP32 DevKit | 1 |
| LED (qualquer cor) | 1 |
| Resistor 220 Ω | 1 |
| Botão (pushbutton) | 2 |
| Resistor 10 kΩ | 2 |

**Conexões:**

```
ESP32 GPIO2  ──► Resistor 220Ω ──► LED (+) ──► GND
ESP32 GPIO4  ──► Botão A ──► 3,3V  |  GPIO4 ──► 10kΩ ──► GND
ESP32 GPIO5  ──► Botão B ──► 3,3V  |  GPIO5 ──► 10kΩ ──► GND
```

---

## 3. Código

### Parte A — AND lógico

```python
from machine import Pin
import time

# Pico: idêntico, ajuste apenas os números dos pinos se necessário
# Pico: btn_a = Pin(14, Pin.IN) | btn_b = Pin(15, Pin.IN) | led = Pin(16, Pin.OUT)

btn_a = Pin(4, Pin.IN)
btn_b = Pin(5, Pin.IN)
led   = Pin(2, Pin.OUT)

print("Operação: AND  (led acende só se A E B forem pressionados)")

while True:
    a = btn_a.value()      # lê bit A
    b = btn_b.value()      # lê bit B
    resultado = a & b      # AND bit a bit

    led.value(resultado)
    print(f"A={a}  B={b}  A&B={resultado}")
    time.sleep(0.2)
```

Teste as quatro combinações de botões e anote os resultados na tabela do Experimento.

---

### Parte B — OR lógico

Troque apenas a linha do operador:

```python
    resultado = a | b      # OR bit a bit
```

---

### Parte C — NOT (inversão segura)

```python
    # ~a em Python retorna inteiro negativo — precisamos mascarar para 1 bit
    resultado = (~a) & 1   # inverte o bit A de forma segura
    # equivalente mais direto:
    # resultado = a ^ 1
```

---

## 4. Experimento

Execute a **Parte A** e preencha a tabela:

### AND — `A & B`

| Botão A | Botão B | `A & B` | LED |
|:-------:|:-------:|:-------:|:---:|
| 0 | 0 | _____ | _____ |
| 0 | 1 | _____ | _____ |
| 1 | 0 | _____ | _____ |
| 1 | 1 | _____ | _____ |

Execute a **Parte B** e preencha:

### OR — `A | B`

| Botão A | Botão B | `A \| B` | LED |
|:-------:|:-------:|:--------:|:---:|
| 0 | 0 | _____ | _____ |
| 0 | 1 | _____ | _____ |
| 1 | 0 | _____ | _____ |
| 1 | 1 | _____ | _____ |

**Perguntas:**

**a)** Qual a diferença de comportamento entre AND e OR quando apenas um botão é pressionado?

> _________________________________________________________________

**b)** Por que `~0` em Python não retorna `1`? O que retorna? Verifique no terminal:

```python
print(~0)    # execute e registre o resultado: _____
print(~1)    # resultado: _____
print((~0) & 1)   # resultado: _____
```

**c)** Em qual situação o resultado de `A & B` e `A | B` é idêntico?

> _________________________________________________________________

---

## 5. Desafio

**Desafio principal:** implemente a operação **XOR** usando apenas `&`, `|` e `~`.

Lembre-se: XOR retorna `1` quando as entradas são **diferentes**.

```python
# Dica: XOR(A, B) = (A OR B) AND NOT(A AND B)
# Escreva em Python:
xor_resultado = ___________________________________________
led.value(xor_resultado)
```

Verifique com as quatro combinações de botões.

**Desafio bônus:** adicione um segundo LED e mostre simultaneamente `A & B` em um e `A | B` no outro. Observe os momentos em que os dois LEDs diferem.

---

## Resumo da aula

- `&` implementa AND: saída `1` apenas quando **todos** os bits são `1`
- `|` implementa OR: saída `1` quando **pelo menos um** bit é `1`  
- `~` inverte bits mas atenção ao sinal em Python — use `(~x) & 1` ou `x ^ 1` para 1 bit
- Operadores bitwise aplicados a `value()` equivalem a **portas lógicas conectadas a pinos reais**

---

*← [Aula 1](./aula01-leitura-escrita-digital.md) | Próxima → [Aula 3: Listas de Pinos e Máscara de Bits](./aula03-listas-mascaras.md)*
