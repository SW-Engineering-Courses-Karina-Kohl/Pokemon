# Laboratório de Java 02 — Herança e Polimorfismo (Tema: Pokémon)
### Duração: 60 minutos
---

## Objetivo de aprendizagem

Ao final deste laboratório, você será capaz de:
- Criar hierarquias de classes usando `extends`
- Usar `super()` para reaproveitar construtores e métodos da superclasse
- Sobrescrever métodos (`@Override`) para implementar polimorfismo
- Usar uma referência de superclasse para armazenar objetos de subclasses diferentes
- Explicar, com suas próprias palavras, a diferença entre sobrecarga e sobrescrita

## Cronograma 

| Tempo | Etapa |
|---|---|
| 0–5 min | Introdução |
| 5–20 min | Parte 1 — Criando a superclasse `Pokemon` |
| 20–35 min | Parte 2 — Criando subclasses com herança |
| 35–50 min | Parte 3 — Polimorfismo com array de `Pokemon` |
| 50–58 min | Parte 4 — Desafio extra (sobrecarga vs. sobrescrita) |
| 58–60 min | Fechamento e perguntas |


## Parte 0 — Preparação (0–5 min)

1. Criem uma pasta chamada `lab-heranca-polimorfismo-pokemon`.
2. Dentro dela, criem um arquivo chamado `Pokemon.java`.

## Parte 1 — Superclasse `Pokemon` (5–20 min)

**Passo 1.1.** No arquivo `Pokemon.java`, digitem exatamente o código abaixo:

```java
public class Pokemon {
    protected String nome;
    protected int nivel;
    protected int hp;

    public Pokemon(String nome, int nivel, int hp) {
        this.nome = nome;
        this.nivel = nivel;
        this.hp = hp;
    }

    public void atacar() {
        System.out.println(nome + " usa Investida e causa dano básico.");
    }

    public void descansar() {
        System.out.println(nome + " descansou e recuperou HP.");
    }

    public String toString() {
        return "Pokemon{nome='" + nome + "', nivel=" + nivel + ", hp=" + hp + "}";
    }
}
```

**Passo 1.2.** Criem um arquivo `TestePokemon.java` na mesma pasta com este conteúdo:

```java
public class TestePokemon {
    public static void main(String[] args) {
        Pokemon p = new Pokemon("MonGenerico", 5, 20);
        p.atacar();
        p.descansar();
        System.out.println(p);
    }
}
```

**Passo 1.3.** Compilem e executem no terminal, dentro da pasta do projeto:

```bash
javac Pokemon.java TestePokemon.java
java TestePokemon
```

**Verificação:** vocês devem ver três linhas de saída no console. Se der erro de compilação, confiram se os nomes dos arquivos são idênticos aos nomes das classes (`Pokemon.java` → `class Pokemon`).

---

## Parte 2 — Subclasses com herança (20–35 min)

**Passo 2.1.** Criem o arquivo `Pikachu.java`:

```java
public class Pikachu extends Pokemon {
    private int cargaEletrica;

    public Pikachu(String nome, int nivel, int hp, int cargaEletrica) {
        super(nome, nivel, hp);
        this.cargaEletrica = cargaEletrica;
    }

    @Override
    public void atacar() {
        System.out.println(nome + " usa Choque do Trovão! Dano elétrico!");
    }
}
```

**Passo 2.2.** Criem o arquivo `Charmander.java`:

```java
public class Charmander extends Pokemon {
    private int temperaturaCauda;

    public Charmander(String nome, int nivel, int hp, int temperaturaCauda) {
        super(nome, nivel, hp);
        this.temperaturaCauda = temperaturaCauda;
    }

    @Override
    public void atacar() {
        System.out.println(nome + " usa Brasas! Dano de fogo!");
    }
}
```

**Passo 2.3.** Atualizem o `TestePokemon.java` para testar as duas subclasses:

```java
public class TestePokemon {
    public static void main(String[] args) {
        Pikachu pikachu = new Pikachu("Pikachu", 10, 35, 100);
        Charmander charmander = new Charmander("Charmander", 8, 32, 60);

        pikachu.atacar();
        pikachu.descansar();

        charmander.atacar();
        charmander.descansar();
    }
}
```

**Passo 2.4.** Compilem e executem novamente:

```bash
javac *.java
java TestePokemon
```

**Pergunta:** por que o método `descansar()` funcionou em `Pikachu` e `Charmander` mesmo sem estar escrito nessas classes? Coloquem como comentário no arquivo TestePokemon.

✅ **Checkpoint:** confirmem que `atacar()` imprime mensagens diferentes para Pikachu e Charmander, mas `descansar()` imprime a mesma mensagem genérica da superclasse.

---

## Parte 3 — Polimorfismo com array de `Pokemon` (35–50 min)

**Passo 3.1.** Substituam o conteúdo de `TestePokemon.java` por:

```java
public class TestePokemon {
    public static void main(String[] args) {
        Pokemon[] time = new Pokemon[3];
        time[0] = new Pokemon("MonGenerico", 5, 20);
        time[1] = new Pikachu("Pikachu", 10, 35, 100);
        time[2] = new Charmander("Charmander", 8, 32, 60);

        for (Pokemon atual : time) {
            atual.atacar();
        }
    }
}
```

**Passo 3.2.** Compilem e executem:

```bash
javac *.java
java TestePokemon
```

**Observação importante:** reparem que a variável `atual` é do tipo `Pokemon`, mas o método chamado depende do tipo real do objeto (`Pikachu` ou `Charmander`) em tempo de execução. Isso é **polimorfismo**.

**Passo 3.3 (mão na massa):** criem uma nova classe `Squirtle` que estende `Pokemon`, com um atributo `nivelDeAgua` (int) e sobrescrevam `atacar()` para imprimir algo como "Squirtle usa Jato d'Água! Dano de água!". Adicionem um `Squirtle` ao array `time` e testem novamente.

✅ **Checkpoint:** o array deve conter 4 Pokémon e cada `atacar()` deve imprimir uma mensagem diferente.

---

## Parte 4 — Desafio extra: sobrecarga x sobrescrita (50–58 min)

Se sobrar tempo, adicionem à classe `Pokemon` um método sobrecarregado (mesmo nome, parâmetros diferentes):

```java
public void atacar(int vezes) {
    for (int i = 0; i < vezes; i++) {
        atacar();
    }
}
```

Testem chamando `atual.atacar(3);` para um dos Pokémon no array e observem que o comportamento polimórfico continua valendo mesmo dentro do método sobrecarregado.

**Reflexão final:** qual é a diferença entre este método novo (sobrecarga) e o `atacar()` que vocês reescreveram em `Pikachu`/`Charmander` (sobrescrita)?

---

## Critérios de entrega

- [ ] Classes `Pokemon`, `Pikachu`, `Charmander` e `Squirtle` compilando sem erros
- [ ] Uso correto de `extends` e `super()`
- [ ] Pelo menos um método sobrescrito com `@Override`
- [ ] Array polimórfico de `Pokemon` funcionando no `main`
- [ ] Resposta escrita às duas perguntas de reflexão
- [ ] tudo zipado no Moodle
