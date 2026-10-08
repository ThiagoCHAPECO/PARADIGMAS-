# Respostas das Atividades — Aula 9

## 1. Para cada trecho: (a) qual é a saída? (b) a decisão foi tomada na compilação ou na execução?

### Java

```java
class Animal {
    String som() { return "..."; }
}

class Cachorro extends Animal {
    String som() { return "au"; }
}

Animal x = new Cachorro();

System.out.println(x.som());
```

**(a) Saída:**

```text
au
```

**(b) Decisão: execução.**

`som()` é um método sobrescrito. Apesar de `x` ser declarado como `Animal`, o objeto real é um `Cachorro`. Portanto, em tempo de execução, Java chama `Cachorro.som()`.

**Regra:** métodos de instância sobrescritos usam **despacho dinâmico (late binding)**.

---

### C++

```cpp
struct A {
    void f() { cout << "A"; }
};

struct B : A {
    void f() { cout << "B"; }
};

B b;
A *p = &b;

p->f();
```

**(a) Saída:**

```text
A
```

**(b) Decisão: compilação.**

Como `f()` não é `virtual`, C++ resolve a chamada com base no tipo do ponteiro (`A*`), e não no tipo real do objeto (`B`).

```cpp
p->f(); // chama A::f()
```

Se `f()` fosse declarado como `virtual` em `A`, a saída seria `B` e a decisão ocorreria em **tempo de execução**.

---

## 2. Para cada trecho: (a) qual é a saída? (b) a decisão foi tomada na compilação ou na execução?

### Java

```java
class A {
    String nome = "A";

    String getNome() {
        return nome;
    }
}

class B extends A {
    String nome = "B";

    String getNome() {
        return nome;
    }
}

A x = new B();

System.out.println(x.nome + " " + x.getNome());
```

**(a) Saída:**

```text
A B
```

**(b) Decisão:**

* `x.nome` → **compilação**
* `x.getNome()` → **execução**

**Por quê?**

`x` tem tipo declarado `A`:

```java
A x = new B();
```

Atributos não usam polimorfismo dinâmico. Portanto, `x.nome` acessa `A.nome`, resultando em `"A"`.

Já `getNome()` é sobrescrito. Como o objeto real é `B`, a execução chama `B.getNome()`, resultando em `"B"`.

**Regra:** atributos → tipo da referência; métodos sobrescritos → tipo real do objeto.

---

### Python

```python
class Contador:
    total = 0

    def __init__(self):
        Contador.total += 1
        self.id = Contador.total

a = Contador()
b = Contador()

print(a.id, b.id, a.total)
```

**(a) Saída:**

```text
1 2 2
```

**(b) Decisão: execução.**

`total` é um atributo da classe, enquanto `id` é um atributo de cada objeto.

Ao criar `a`:

```text
Contador.total = 1
a.id = 1
```

Ao criar `b`:

```text
Contador.total = 2
b.id = 2
```

Como `a` não possui um atributo próprio `total`, `a.total` encontra `Contador.total`, que vale `2`.

**Resultado:**

```text
a.id    → 1
b.id    → 2
a.total → 2
```

**Regra:** `total` pertence à classe; `id` pertence a cada instância. A resolução desses atributos ocorre em **tempo de execução**.

---

## 3. Para cada trecho: (a) qual é a saída? (b) a decisão foi tomada na compilação ou na execução?

### Java — Métodos estáticos

```java
class A {
    static String quem() {
        return "A";
    }
}

class B extends A {
    static String quem() {
        return "B";
    }
}

A x = new B();

System.out.println(x.quem());
```

**(a) Saída:**

```text
A
```

**(b) Decisão: compilação.**

`quem()` é um método `static`. Métodos estáticos não participam do polimorfismo dinâmico.

Como `x` foi declarado como `A`:

```java
A x = new B();
```

a chamada:

```java
x.quem();
```

é resolvida de acordo com o **tipo declarado da referência**, `A`.

Portanto, chama `A.quem()`.

**Regra:** métodos `static` são resolvidos pelo tipo da referência, não pelo tipo real do objeto.

---

### Go — Composição e métodos

```go
type Animal struct{}

func (Animal) Som() string {
    return "..."
}

func (a Animal) Falar() string {
    return "faz " + a.Som()
}

type Cao struct {
    Animal
}

func (Cao) Som() string {
    return "au"
}

fmt.Println(Cao{}.Falar(), Cao{}.Som())
```

**(a) Saída:**

```text
faz ... au
```

**(b) Decisão: execução.**

`Cao` possui `Animal` por **composição/embedding**. Assim, `Cao` promove os métodos de `Animal`, podendo chamar:

```go
Cao{}.Falar()
```

Porém, `Falar()` foi definido em `Animal`:

```go
func (a Animal) Falar() string {
    return "faz " + a.Som()
}
```

Dentro desse método, `a` é um `Animal`, não um `Cao`. Portanto:

```go
a.Som()
```

chama `Animal.Som()`, produzindo `"..."`.

Já:

```go
Cao{}.Som()
```

chama diretamente `Cao.Som()`, produzindo `"au"`.

**Resultado:**

```text
Cao{}.Falar() → "faz ..."
Cao{}.Som()   → "au"
```

**Regra:** o embedding promove métodos, mas não cria sobrescrita polimórfica como em Java. O receptor definido no método determina qual método é chamado.

---

### Resumo das regras

| Linguagem | Situação                        | Decisão                       |
| --------- | ------------------------------- | ----------------------------- |
| Java      | Método de instância sobrescrito | Execução                      |
| C++       | Método não `virtual`            | Compilação                    |
| Java      | Atributo                        | Compilação                    |
| Java      | Método `static`                 | Compilação                    |
| Python    | Acesso a atributos              | Execução                      |
| Go        | Método via embedding            | Execução, conforme o receptor |
