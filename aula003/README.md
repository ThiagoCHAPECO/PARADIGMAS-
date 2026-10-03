# Atividade — Derivação de um Código a partir da Gramática de uma Linguagem de Programação

## 1. Linguagem escolhida

A linguagem de programação escolhida para esta atividade foi **Java**.

O trecho utilizado apresenta uma classe com método `main`, declaração de variável, uma estrutura de repetição `for` e uma estrutura condicional `if`.

---

## 2. Fonte da gramática

A gramática utilizada foi consultada na documentação oficial da linguagem Java, por meio da **Java Language Specification (JLS)**, disponibilizada pela Oracle.

**Fonte:**
https://docs.oracle.com/en/java/javase/26/docs/specs/jls/index.html

Foram utilizadas principalmente as regras relacionadas a:

* `Statement`;
* `ForStatement`;
* `BasicForStatement`;
* `IfThenStatement`;
* `Block`;
* `Expression`.

---

## 3. Notação utilizada

A especificação da linguagem Java utiliza uma **A gramática da linguagem Java é especificada por meio de uma notação formal própria baseada em produções gramaticais. A JLS utiliza uma forma de notação semelhante à BNF, com algumas extensões para representar as construções da linguagem.**, apresentando suas regras por meio de produções.

As produções são utilizadas para representar como os elementos da linguagem podem ser combinados para formar programas Java sintaticamente válidos.

Por exemplo, para o `if`, temos:

```text
IfThenStatement:
    if ( Expression ) Statement
```

Essa produção indica que uma instrução `if` é formada pela palavra-chave `if`, seguida de uma expressão entre parênteses e de uma instrução.

---

## 4. Produções selecionadas

Foram selecionadas somente as produções necessárias para representar as principais estruturas do código.

### Statement

```text
Statement:
    StatementWithoutTrailingSubstatement
    LabeledStatement
    IfThenStatement
    IfThenElseStatement
    WhileStatement
    ForStatement
```

Essa produção permite representar diferentes tipos de instruções da linguagem Java.

### ForStatement

```text
ForStatement:
    BasicForStatement
    EnhancedForStatement
```

Como o código utiliza um `for` tradicional, foi selecionado `BasicForStatement`.

### BasicForStatement

```text
BasicForStatement:
    for ( [ForInit] ; [Expression] ; [ForUpdate] ) Statement
```

Essa produção representa a estrutura do `for`, contendo inicialização, condição, atualização e a instrução que será executada.

### IfThenStatement

```text
IfThenStatement:
    if ( Expression ) Statement
```

Essa produção representa o `if` sem uma cláusula `else`.

### Block

```text
Block:
    { [BlockStatements] }
```

Essa produção representa um bloco de código delimitado por `{` e `}`.

---

## 5. Código escolhido

O código utilizado para a atividade foi:

```java
public class AtividadeDerivacao {

    public static void main(String[] args) {
        int cont;
        for (cont = 0; cont < 5; cont++){
            if (cont % 2 == 0){
                System.out.println(cont+": PAR!!!");
            }
        }
    }
}
```
## 6. Derivação

A derivação parte de um símbolo não terminal e aplica sucessivamente as produções da gramática até chegar ao código correspondente.

### Derivação do `for`

```text
<Statement>
⇒ <ForStatement>
⇒ <BasicForStatement>
⇒ for ( <ForInit> ; <Expression> ; <ForUpdate> ) <Statement>

⇒ for ( <StatementExpressionList> ; <Expression> ; <ForUpdate> ) <Statement>

⇒ for ( <StatementExpression> ; <Expression> ; <ForUpdate> ) <Statement>

⇒ for ( <Assignment> ; <Expression> ; <ForUpdate> ) <Statement>

⇒ for ( cont = 0 ; <Expression> ; <ForUpdate> ) <Statement>

⇒ for ( cont = 0 ; cont < 5 ; <ForUpdate> ) <Statement>

⇒ for ( cont = 0 ; cont < 5 ; cont++ ) <Statement>

⇒ for ( cont = 0 ; cont < 5 ; cont++ ) <IfThenStatement>

⇒ for ( cont = 0 ; cont < 5 ; cont++ )
   if ( <Expression> ) <Statement>

⇒ for ( cont = 0 ; cont < 5 ; cont++ )
   if ( cont % 2 == 0 ) <Block>

⇒ for ( cont = 0 ; cont < 5 ; cont++ )
   if ( cont % 2 == 0 ) { <BlockStatements> }

⇒ for ( cont = 0 ; cont < 5 ; cont++ )
   if ( cont % 2 == 0 ) { <BlockStatement> }

⇒ for ( cont = 0 ; cont < 5 ; cont++ )
   if ( cont % 2 == 0 ) { <Statement> }

⇒ for ( cont = 0 ; cont < 5 ; cont++ )
   if ( cont % 2 == 0 ) { <ExpressionStatement> }

⇒ for ( cont = 0 ; cont < 5 ; cont++ )
   if ( cont % 2 == 0 ) { <StatementExpression> ; }

⇒ for ( cont = 0 ; cont < 5 ; cont++ )
   if ( cont % 2 == 0 ) { <MethodInvocation> ; }

⇒ for ( cont = 0 ; cont < 5 ; cont++ )
   if ( cont % 2 == 0 ) {
       System.out.println(cont + ": PAR!!!");
   }
```

---

## 7. Terminais e não terminais

### Símbolos terminais

Os símbolos terminais são os elementos que aparecem efetivamente no código final.

Exemplos utilizados no código:

```text
for
if
(
)
{
}
;
=
<
%
==
++
int
public
class
static
void
cont
0
2
5
```

Também são considerados terminais os identificadores, literais e demais tokens presentes no código.

### Símbolos não terminais

Os símbolos não terminais representam estruturas da gramática e são substituídos durante o processo de derivação.

Os principais utilizados nesta atividade são:

```text
Statement
ForStatement
BasicForStatement
ForInit
StatementExpressionList
StatementExpression
ForUpdate
IfThenStatement
Expression
Block
BlockStatements
```

Esses símbolos não aparecem no código Java final. Eles são utilizados durante o processo de derivação.

---

## 8. Relação entre as produções e o código

As principais relações entre as regras e o código são:

| Produção            | Trecho correspondente              |
| ------------------- | ---------------------------------- |
| `ForStatement`      | `for (...)`                        |
| `BasicForStatement` | `for (cont = 0; cont < 5; cont++)` |
| `ForInit`           | `cont = 0`                         |
| `Expression`        | `cont < 5`                         |
| `ForUpdate`         | `cont++`                           |
| `IfThenStatement`   | `if (cont % 2 == 0)`               |
| `Block`             | `{ ... }`                          |
| `Expression`        | `cont % 2 == 0`                    |

---

## 9. Resultado final

Após a aplicação das produções selecionadas, é possível obter o seguinte trecho:

```java
for (cont = 0; cont < 5; cont++){
    if (cont % 2 == 0){
        System.out.println(cont+": PAR!!!");
    }
}
```

O resultado demonstra que as produções selecionadas da gramática Java são capazes de representar a estrutura sintática utilizada no código.

---

## 10. Conclusão

A atividade permitiu relacionar os conceitos de gramática formal com uma linguagem de programação real.

A partir da gramática oficial da linguagem Java, foram selecionadas produções relacionadas às estruturas `for`, `if`, blocos e expressões. Durante a derivação, os símbolos não terminais foram substituídos sucessivamente pelas produções correspondentes até chegar à estrutura concreta do código.

Dessa forma, foi possível observar como uma gramática formal pode representar a sintaxe de uma linguagem de programação e como um código válido pode ser obtido por meio da aplicação sucessiva de suas regras de produção.
