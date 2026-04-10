# O “truque” de descobrir o dia da semana de qualquer data

Em algum momento da vida, quase todo mundo já viu alguém fazer a seguinte “mágica”:

> “Fale uma data qualquer… e eu te digo o dia da semana.”

À primeira vista, parece memorização absurda ou algum tipo de talento especial. Mas a verdade é mais interessante: trata-se de um problema matemático elegante, com solução determinística e eficiente.

Neste texto, vamos entender como isso funciona — e, ao final, você terá implementações completas em várias linguagens.

---

# Parte 1 — A lógica por trás do calendário

O calendário que usamos hoje é baseado em ciclos:

* A semana tem 7 dias
* Os meses têm durações fixas (com pequenas variações)
* Os anos seguem regras previsíveis (incluindo anos bissextos)

Isso permite transformar qualquer data em um número que representa um dia da semana.

Esse tipo de transformação é estudado dentro de conceitos como:

👉 Calendário Gregoriano

👉 Congruência de Zeller

---

# Parte 2 — A fórmula que resolve o problema

Uma das formas mais conhecidas de resolver isso é a **Congruência de Zeller**.

Ela pega dia, mês e ano e devolve um número de 0 a 6, representando o dia da semana.

h = \left(q + \left\lfloor \frac{13(m+1)}{5} \right\rfloor + K + \left\lfloor \frac{K}{4} \right\rfloor + \left\lfloor \frac{J}{4} \right\rfloor + 5J\right) \bmod 7

Onde:

* q = dia do mês
* m = mês (março = 3 até fevereiro = 14)
* K = ano dentro do século
* J = século

Resultado:

* 0 = sábado
* 1 = domingo
* 2 = segunda
* 3 = terça
* 4 = quarta
* 5 = quinta
* 6 = sexta

---

# Parte 3 — Um detalhe importante: o mês

Janeiro e fevereiro são tratados como meses 13 e 14 do ano anterior.

Exemplo:

* Janeiro de 2025 vira mês 13 de 2024
* Fevereiro de 2025 vira mês 14 de 2024

Isso simplifica a lógica interna da fórmula.

---

# Parte 4 — E antes de 1582?

Aqui entra um detalhe histórico importante.

Em 1582 ocorreu a:

👉 Reforma do Calendário Gregoriano

Antes disso, era usado o:

👉 Calendário Juliano

Na prática:

* Datas antigas podem ter resultados diferentes dependendo do calendário adotado
* Sistemas modernos normalmente usam o:
  👉 Calendário Gregoriano Proléptico

Ou seja: aplicam a mesma regra para todas as datas, garantindo consistência.

---

# Parte 5 — Implementações

A seguir, a mesma lógica implementada em diferentes linguagens.

---

## Python

```python
def dia_semana(d, m, a):
    if m < 3:
        m += 12
        a -= 1

    K = a % 100
    J = a // 100

    h = (d + (13 * (m + 1)) // 5 + K + (K // 4) + (J // 4) + (5 * J)) % 7

    dias = ["Sabado", "Domingo", "Segunda", "Terca", "Quarta", "Quinta", "Sexta"]
    return dias[h]

print(dia_semana(15, 3, 2025))
```

---

## Shell (bash)

```bash
#!/bin/bash

dia=$1
mes=$2
ano=$3

if [ $mes -lt 3 ]; then
  mes=$((mes + 12))
  ano=$((ano - 1))
fi

K=$((ano % 100))
J=$((ano / 100))

h=$(( (dia + (13 * (mes + 1)) / 5 + K + K/4 + J/4 + 5*J) % 7 ))

dias=("Sabado" "Domingo" "Segunda" "Terca" "Quarta" "Quinta" "Sexta")

echo ${dias[$h]}
```

---

## Java

```java
public class Zeller {

    public static String diaSemana(int d, int m, int a) {
        if (m < 3) {
            m += 12;
            a -= 1;
        }

        int K = a % 100;
        int J = a / 100;

        int h = (d + (13 * (m + 1)) / 5 + K + (K / 4) + (J / 4) + (5 * J)) % 7;

        String[] dias = {
            "Sabado", "Domingo", "Segunda",
            "Terca", "Quarta", "Quinta", "Sexta"
        };

        return dias[h];
    }

    public static void main(String[] args) {
        System.out.println(diaSemana(15, 3, 2025));
    }
}
```

---

## C

```c
#include <stdio.h>

const char* dias[] = {
    "Sabado", "Domingo", "Segunda",
    "Terca", "Quarta", "Quinta", "Sexta"
};

const char* diaSemana(int d, int m, int a) {
    if (m < 3) {
        m += 12;
        a -= 1;
    }

    int K = a % 100;
    int J = a / 100;

    int h = (d + (13 * (m + 1)) / 5 + K + (K / 4) + (J / 4) + (5 * J)) % 7;

    return dias[h];
}

int main() {
    printf("%s\n", diaSemana(15, 3, 2025));
    return 0;
}
```

---

## Pascal

```pascal
program Zeller;

uses SysUtils;

function DiaSemana(d, m, a: integer): string;
var
  K, J, h: integer;
  dias: array[0..6] of string;
begin
  dias[0] := 'Sabado';
  dias[1] := 'Domingo';
  dias[2] := 'Segunda';
  dias[3] := 'Terca';
  dias[4] := 'Quarta';
  dias[5] := 'Quinta';
  dias[6] := 'Sexta';

  if m < 3 then
  begin
    m := m + 12;
    a := a - 1;
  end;

  K := a mod 100;
  J := a div 100;

  h := (d + (13 * (m + 1)) div 5 + K + (K div 4) + (J div 4) + (5 * J)) mod 7;

  DiaSemana := dias[h];
end;

begin
  writeln(DiaSemana(15, 3, 2025));
end.
```

---

## BASIC

```basic
FUNCTION DiaSemana$(d, m, a)
    IF m < 3 THEN
        m = m + 12
        a = a - 1
    END IF

    K = a MOD 100
    J = a \ 100

    h = (d + (13 * (m + 1)) \ 5 + K + (K \ 4) + (J \ 4) + (5 * J)) MOD 7

    DIM dias$(6)
    dias$(0) = "Sabado"
    dias$(1) = "Domingo"
    dias$(2) = "Segunda"
    dias$(3) = "Terca"
    dias$(4) = "Quarta"
    dias$(5) = "Quinta"
    dias$(6) = "Sexta"

    DiaSemana$ = dias$(h)
END FUNCTION

PRINT DiaSemana$(15, 3, 2025)
```

---

# Conclusão

A “mágica” não é mágica: é matemática aplicada com elegância.

Com uma fórmula simples e algumas operações inteiras, conseguimos:

* Determinar o dia da semana de qualquer data
* Implementar isso em qualquer linguagem
* Garantir consistência e performance

Esse tipo de problema mostra bem como conceitos matemáticos aparentemente abstratos têm aplicações diretas e práticas — inclusive em sistemas reais de alta escala.
