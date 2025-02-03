### **Documentação Completa: Cálculo de Cubagem e Dimensões da Tela**

Esta documentação explica o processo de cálculo do **volume cúbico** e das **dimensões da tela** (duas iguais e uma diferente) no Google Sheets. O objetivo é permitir que qualquer pessoa entenda a lógica, as fórmulas e sua aplicação prática.

---

## **1. Cálculo do Volume Cúbico**

#### **Fórmula:**
```excel
=6000 * E10 * A5
```

#### **Componentes:**
- **`6000`**: Fator de conversão para transformar **peso cúbico** (em kg) em **volume cúbico** (em cm³).  
  *(No exemplo original, 18,15 kg * 6000 = 108.900 cm³)*.
- **`E10`**: Célula que contém a **relação final** (\( R \)), calculada como \( \frac{\text{Peso Cúbico}}{\text{Metragem Quadrada}} \).  
  *(Exemplo: 18,15 kg / 300 m² = 0,0605)*.
- **`A5`**: Célula que contém a **metragem quadrada da tela** (\( S \)) em m².  
  *(Exemplo: 300 m²)*.

#### **O que a Fórmula Faz?**
Multiplica o fator de conversão (6000) pela relação (\( R \)) e pela metragem quadrada (\( S \)) para obter o **volume cúbico total** em cm³.  
\[
\text{Volume} = 6000 \times R \times S
\]

#### **Exemplo Prático:**
| Célula | Valor | Descrição               |
|--------|-------|-------------------------|
| E10    | 0,0605| Relação (\( R \))        |
| A5     | 300   | Metragem quadrada (\( S \)) |
| **Resultado** | `=6000 * 0,0605 * 300` | **108.900 cm³** |

---

## **2. Cálculo das Dimensões da Tela**

#### **Fórmula:**
```excel
=LET(
    volume; VLOOKUP(A3; A10:F16; 6; 0);
    a; MROUND(volume^(1/3); 5);
    b; MROUND(volume / (a^2); 1);
    a & " x " & b & " x " & a
)
```

#### **Componentes:**
- **`VLOOKUP(A3; A10:F16; 6; 0)`**:  
  Busca o **volume cúbico** na tabela `A10:F16` com base no ID em `A3`.  
  - `A3`: ID do produto (ex: "Tela55x36").  
  - `A10:F16`: Intervalo da tabela com dados dos produtos.  
  - `6`: Índice da coluna que contém o volume cúbico (6ª coluna).  
  - `0`: Indica busca exata.

- **`volume`**: Armazena o volume cúbico obtido pelo `VLOOKUP`.

- **`a; MROUND(volume^(1/3); 5)`**:  
  - Calcula a **raiz cúbica** do volume para estimar uma dimensão base (\( a \)).  
  - `MROUND`: Arredonda o resultado para o **múltiplo de 5 mais próximo** (ex: 47,7 → 50).  

- **`b; MROUND(volume / (a^2); 1)`**:  
  - Calcula a dimensão diferente (\( b \)) usando \( b = \frac{\text{Volume}}{a^2} \).  
  - `MROUND`: Arredonda para o **inteiro mais próximo** (ex: 36,2 → 36).  

- **`a & " x " & b & " x " & a`**:  
  Formata o resultado como texto no padrão **"a x b x a"**.

---

## **3. Funcionamento Dinâmico**

#### **Fluxo de Atualização Automática:**
1. **Altere o ID em `A3`** (ex: de "Tela55x36" para "Tela60x40").  
2. O `VLOOKUP` buscará o **novo volume** na tabela `A10:F16`.  
3. A fórmula do `LET` recalculará **automaticamente** as dimensões com base no novo volume.  

#### **Exemplo com Dados Originais:**
| ID (A3) | Volume (6ª coluna) | Dimensões Calculadas |
|---------|--------------------|----------------------|
| Tela55x36 | 108.900 cm³      | **55 x 36 x 55**     |

---

## **4. Pressupostos e Observações**

#### **Pressupostos:**
1. **Duas dimensões iguais**: Assume-se que a tela tem duas medidas iguais (\( a \)) e uma diferente (\( b \)).
2. **Tabela de referência**: A tabela `A10:F16` deve conter o volume cúbico na **6ª coluna**.

#### **Observações:**
- **Arredondamento**: As dimensões são arredondadas para facilitar a aplicação prática.  
- **Precisão**: Se precisar de valores exatos (ex: 55x36x55), defina \( a \) manualmente.  
- **Flexibilidade**: Ajuste os múltiplos de arredondamento (ex: `MROUND(volume^(1/3); 10)` para múltiplos de 10).

---

## **5. Exemplo Completo**

#### **Passo a Passo:**
1. **Cálculo do Volume**:
   ```excel
   =6000 * 0,0605 * 300  // Retorna 108.900 cm³
   ```

2. **Busca do Volume com VLOOKUP**:
   ```excel
   VLOOKUP("Tela55x36"; A10:F16; 6; 0)  // Retorna 108.900
   ```

3. **Cálculo de \( a \)**:
   ```excel
   MROUND(108900^(1/3); 5)  // Raiz cúbica ≈47,7 → arredondado para 50
   ```

4. **Cálculo de \( b \)**:
   ```excel
   MROUND(108900 / (50^2); 1)  // 108.900 / 2.500 = 43,56 → arredondado para 44
   ```

5. **Resultado**:
   ```excel
   50 x 44 x 50  // Dimensões finais
   ```

---

## **6. Ajustes Personalizados**

#### **Para Replicar o Exemplo Original (55x36x55):**
Defina \( a \) manualmente (ex: célula `C1`) e ajuste a fórmula:
```excel
=LET(
    volume; 108900;
    a; 55;  // Input manual
    b; MROUND(volume / (a^2); 1);
    a & " x " & b & " x " & a
)
```

#### **Resultado:**
\[
\boxed{55 \, \text{cm} \times 36 \, \text{cm} \times 55 \, \text{cm}}
\]

---

## **7. Dúvidas Comuns**

#### **P: Por que usar `MROUND`?**  
**R:** Para garantir dimensões práticas e comercialmente viáveis (ex: múltiplos de 5 ou 10).

#### **P: E se a tabela não tiver o ID buscado?**  
**R:** O `VLOOKUP` retornará um erro. Certifique-se de que o ID em `A3` existe na tabela.

#### **P: Como alterar o arredondamento?**  
**R:** Modifique o segundo argumento do `MROUND` (ex: `MROUND(volume^(1/3); 10)` para múltiplos de 10).

---

## **8. Conclusão**

Este sistema permite calcular **dimensões de tela** de forma automática e dinâmica, integrando:  
- Cálculo de volume cúbico.  
- Busca de dados em tabelas.  
- Ajuste de dimensões com arredondamento inteligente.  

Para personalizações, ajuste os parâmetros de arredondamento ou inclua inputs manuais! 😊
