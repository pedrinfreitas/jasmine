# Jasmine
## O que é:
Framework para criação de testes, utilza BDD (Behavior Driven Development - Testes guiados por comportamento)
* testes intuitivos e de facil compreesão.
* É rapido, não possui dependencias externas.
* inclui tudo que é necessario para realizar testes.
* pode ser executado pelo navegador ou pelo terminal.
* facil instalação e configuração.
* pode ser usado de forma independente e em projetos nodeJS, Ruby ou Python.

Testando helloWorld:
```javascript
function helloWorld() {
  return 'Hello world!';
}
```

```javascript
1  describe('Hello world', () => {
2    it('says hello', () => {
3      expect(helloWorld())
4        .toEqual('Hello world!');
5    });
6  });
```
Linha 1 -  ``` describe(string, () => { } ) ``` define o que chamamos de  Suite de Testes, uma coleção de Especificações de Teste individuais.

Linha 2 -  ``` it(string, () => { }) ``` define uma especificação de teste individual, que contém uma ou mais expectativas de teste.

Linha 3 -  ``` expect(actual) ``` define uma expectativa, em conjunto com um  Matcher, ele descreve um  comportamento esperado no aplicativo.

Linha 4 -  ``` matcher(expected) ``` define uma comparação booleana com o valor esperado (algums precisam de parametros).

#### Alguns Matchers
```javascript
expect(array).toContain(member);
expect(fn).toThrow(string);
expect(fn).toThrowError(string);
expect(instance).toBe(instance);
expect(mixed).toBeDefined();
expect(mixed).toBeFalsy();
expect(mixed).toBeNull();
expect(mixed).toBeTruthy();
expect(mixed).toBeUndefined();
expect(mixed).toEqual(mixed);
expect(mixed).toMatch(pattern);
expect(number).toBeCloseTo(number, decimalPlaces);
expect(number).toBeGreaterThan(number);
expect(number).toBeLessThan(number);
expect(number).toBeNaN();
expect(spy).toHaveBeenCalled();
expect(spy).toHaveBeenCalledTimes(number);
expect(spy).toHaveBeenCalledWith(...arguments);
```

### beforeAll
Essa função é chamada uma vez antes que todas as especificações em um conjunto de testes sejam executadas.

### afterAll
Essa função é chamada uma vez depois que todas as especificações em um conjunto de testes sejam executadas.

### beforeEach
Essa função é chamada antes de cada especificação de teste ``` it(string, () => { }) ``` ser executada.

### afterEach
Essa função é chamada depois de cada especificação de teste ``` it(string, () => { }) ``` ser executada.

#### Podemos usar essas funções da seguinte maneira:

```javascript
describe('Hello world', () => {

  let expected = "";

  beforeEach(() => {
    expected = "Hello World";
  });

  afterEach(() => {
    expected = "";
  });

  it('says hello', () => {
    expect(helloWorld())
        .toEqual(expected);
  });
});
```

## Desativar Teste
Você pode desativar teste sem comentá-lo, basta colocar um x no describe ou it, da seguinte maneira: (linhas 1 e 2)
```javascript
1  xdescribe('Hello world', () => {
2    xit('says hello', () => {
3      expect(helloWorld())
5        .toEqual('Hello world!');
5    });
6  });
```



## Focar/Priorizar Teste
Você pode Focar/Priorizar, basta colocar um f no describe ou it, da seguinte maneira: (linhas 1 e 2) 
```javascript
1  fdescribe('Hello world', () => {
2    fit('says hello', () => {
3      expect(helloWorld())
5        .toEqual('Hello world!');
5    });
6  });
```
>De todos os testes apenas esse será executado.

## Exemplo de teste em service
Vamos imaginar a seguinte classe:
```javascript
export class AuthService {
  isAuthenticated(): boolean {
    return !!localStorage.getItem('token');
  }
}
```
Ele possui uma função chamada isAuthenticatedque retorna truese houver um token armazenado nos navegadores localStorage.

Para testar essa classe, criamos um arquivo de teste chamado ```auth.service.spec.ts``` que fica ao lado do nosso ``` auth.service.ts ```, assim:

```javascript
1  import {AuthService} from './auth.service';
2  
3  describe('Service: Auth', () => { });
```
linha 1 - Importamos a class ```AuthService``` na qual queremos executar nossos testes.
linha 2 - Adicionamos um ```describle``` para armazenar todas as nossas especificações de teste individuais.

### Setup
Queremos executar nossas especificações de teste em ```AuthService```, para isso vamos usar ```beforeEache``` e  ```afterEach``` para configurar e limpar as instâncias da seguinte forma:

```javascript 
01  describe('Service: Auth', () => {
02    let service: AuthService;
03
04    beforeEach(() => {
05      service = new AuthService();
06    });
07
08    afterEach(() => {
09      service = null;
10      localStorage.removeItem('token');
11    });
12  });
```
linha 04 - Antes de cada especificação de teste ser executada(``it``), criamos uma nova instância ``AuthService`` e armazenamos na variável service.

linha 08 - Após a conclusão de cada especificação de teste(``it``), anulamos nosso service e também removemos todos os tokens armazenados em localStorage.

### Criando especificação de teste
A primeira especificação que vamos criar deve verificar se ``isAuthenticated`` função retorna true`quando há um token.

```javascript
1  it('should return true from isAuthenticated when there is a token', () => { 
2    localStorage.setItem('token', '1234');
3    expect(service.isAuthenticated()).toBeTruthy();
4  });
```

linha 1 - Passamos para o it uma descrição legível, o que facilita muito para ver erros no relatorio gerado.
linha 2 - Configuramos algumas especificações de dados no armazenamento local, que devem acionar o efeito que queremos.
linha 3 - Testamos uma expectativa de que a ``service.isAuthenticated()`` retorne true ``.toBeTruthy()``.

Também queremos testar o caso inverso, quando não houver token, a função deve retornar ``false``:

```javascript
1  it('should return false from isAuthenticated when there is no token', () => {
2    expect(service.isAuthenticated()).toBeFalsy();
3  });
```

Nós sabemos que ``token`` não está definido, ja que fizemos isso no ``afterEach``.

Agora testamos uma expectativa de que a  ``service.isAuthenticated()`` retorne algo que resulte em ``false``.

## Pipes
Um exemplo de codigo:
```javascript
import {Pipe, PipeTransform} from '@angular/core';

@Pipe({
  name: 'default'
})
export class DefaultPipe implements PipeTransform {

  transform(value: string, fallback: string, forceHttps: boolean = false): string {
    let image = "";
    if (value) {
      image = value;
    } else {
      image = fallback;
    }
    if (forceHttps) {
      if (image.indexOf("https") == -1) {
        image = image.replace("http", "https");
      }
    }
    return image;
  }
}
```
O teste:
```javascript
describe('Pipe: Default', () => {
  let pipe: DefaultPipe;

  beforeEach(() => {
    pipe = new DefaultPipe();
  });
});
```

Em nossa função de configuração, criamos uma instância da nossa classe de pipe, que tem uma função chamada transform, devemos apenas testar essa função, passando entradas e esperando saídas.

Nossa primeira especificação de teste verifica se, não recebe uma entrada, ele retorna o valor padrão, assim:

```javascript
it('providing no value returns fallback', () => {
  expect(pipe.transform('', 'http://place-hold.it/300')).toBe('http://place-hold.it/300');
});
```
Passamos uma string vazia como entrada para a função transform, portanto, ela retorna o segundo argumento para nós.

Para testar pipes, não há muito mais, simplesmente verificamos as várias entradas e saídas esperadas.

>Se o seu Pipe exigir que as dependências sejam injetadas no construtor, talvez seja melhor usar o  ```Angular Test Bed``` que abordaremos mais adiante.
`

## Testes com Mocks e Spies
Exemplo e codigo:
```javascript
import {Component} from '@angular/core';
import {AuthService} from "./auth.service";

@Component({
  selector: 'app-login',
  template: `<a [hidden]="needsLogin()">Login</a>`
})

export class LoginComponent {

  constructor(private auth: AuthService) {
  }

  needsLogin() {
    return !this.auth.isAuthenticated();
  }
}
```

Injetamos o  ``AuthService`` no  ``LoginComponent`` e o componente mostra um  botão Login se ``AuthService`` não estiver logado.

O  ``AuthService``:
```javascript
export class AuthService {
  isAuthenticated(): boolean {
    return !!localStorage.getItem('token');
  }
}
```
### Testamdo o ``AuthService`` real
```javascript
01  import {LoginComponent} from './login.component';
02  import {AuthService} from "./auth.service";
03
04  describe('Component: Login', () => {
05
06    let component: LoginComponent;
07    let service: AuthService;
08
09    beforeEach(() => {
10      service = new AuthService();
11      component = new LoginComponent(service);
12    });
13
14    afterEach(() => {
15      localStorage.removeItem('token');
16      service = null;
17      component = null;
18    });
19
20
21    it('needsLogin returns true when the user has not been authenticated',22 () => {
23      expect(component.needsLogin()).toBeTruthy();
24    });
25
26    it('needsLogin returns false when the user has been authenticated', ()27 => {
28      localStorage.setItem('token', '12345');
29      expect(component.needsLogin()).toBeFalsy();
30    });
31  });
```
linha 09 - Criamos uma instância  ``AuthService`` e a injetamos ``LoginComponent`` quando criamos.
linha 14 - Limpamos os dados e o localStorage após a execução de cada especificação de teste.
linha 28 - Configuramos alguns dados no ``localStorage`` para obter o comportamento que queremos em ``AuthService``.

Portanto, para testar ``LoginComponent``, precisamos conhecer o  funcionamento interno de  ``AuthService``, imagine se fosse necessário várias outras dependências para executar, precisaríamos conhecer o funcionamento interno de várias outras classes apenas para testar ``LoginComponent``.

### Mock com classes falsas
Podemos criar um mock de ``AuthService`` como ``MockedAuthService`` que apenas retorna o que queremos para o nosso teste.

Podemos até remover a importação de ``AuthService``, O ``LoginComponent`` é testado isoladamente:

```javascript
01  import {LoginComponent} from './login.component';
02
03  class MockAuthService {
04    authenticated = false;
05
06    isAuthenticated() {
07      return this.authenticated;
08    }
09  }
10
11  describe('Component: Login', () => {
12
13    let component: LoginComponent;
14    let service: MockAuthService;
15
16    beforeEach(() => {
17      service = new MockAuthService();
18      component = new LoginComponent(service);
19    });
20
21    afterEach(() => {
22      service = null;
23      component = null;
24    });
25
26
27    it('needsLogin returns true when the user has not been authenticated', () => {
28      service.authenticated = false;
29      expect(component.needsLogin()).toBeTruthy();
30    });
31
32    it('needsLogin returns false when the user has been authenticated', () => {
33      service.authenticated = true;
34      expect(component.needsLogin()).toBeFalsy();
35    });
36  });
```
**linha 03** - Criamos uma classe chamada ``MockAuthService`` que tem a mesma função ``isAuthenticated`` que a classe ``AuthService`` real. A única diferença é que podemos controlar o que ``isAuthenticated`` retorna definindo o valor da ``authenticated``.
**linha 16**  - Injetamos em nosso ``LoginComponent`` um ``MockAuthService`` no lugar do ``AuthService`` real.
**linhas 28 e 33** - Em nossos testes, acionamos o comportamento que queremos do ``service``, definindo a  propriedade ``authenticated``.

Usando um Mock, não dependemos da classe real, na verdade, nem precisamos importá-lo para nossas especificações.

### Mock com funções
Às vezes, criar um mock completo de uma classe pode ser complicado, demorado e desnecessário.

Em vez disso, podemos simplesmente estender a classe e substituir uma ou mais funções específicas para fazer com que elas retornem as respostas de teste necessárias, da seguinte forma:

```javascript
class MockAuthService extends AuthService {
  authenticated = false;

  isAuthenticated() {
    return this.authenticated;
  }
}
```
Na classe acima ``MockAuthService`` se estende ``AuthService``. Ele teria acesso a todas as outras funções e propriedades, ``AuthService`` apenas substitui a  ``isAuthenticated`` para que possamos controlar facilmente seu comportamento e isolar nosso ``LoginComponent``.


### Mock com Spy
Um Spy é um recurso do Jasmine que permite pegar uma classe, função ou objeto existente e zombar de tal maneira que você possa controlar o que é retornado pela chamada da função.

Vamos reescrever nosso teste para usar um Spy em uma instância real ``AuthService``, como:
```javascript
01  import {LoginComponent} from './login.component';
02  import {AuthService} from "./auth.service";
03
04  describe('Component: Login', () => {
05
06    let component: LoginComponent;
07    let service: AuthService;
08    let spy: any;
09
10    beforeEach(() => {
11      service = new AuthService();
12      component = new LoginComponent(service);
13    });
14
15    afterEach(() => {
16      service = null;
17      component = null;
18    });
19
20
21    it('needsLogin returns true when the user has not been authenticated', () => {
22      spy = spyOn(service, 'isAuthenticated').and.returnValue(false);
23      expect(component.needsLogin()).toBeTruthy();
24      expect(service.isAuthenticated).toHaveBeenCalled();
25    });
26
27    it('needsLogin returns false when the user has been authenticated', () => {
28      spy = spyOn(service, 'isAuthenticated').and.returnValue(true);
29      expect(component.needsLogin()).toBeFalsy();
30      expect(service.isAuthenticated).toHaveBeenCalled();
31    });
32  });
```

**linha 10** Criamos uma instância real ``AuthService`` e a injetamos no  LoginComponent.


----

## Como criar suites e testes:

## Como validar e comparar resultados:

## Como criar Mocks (spies):

## jasmine clock:

## Comparador personalizado:

## Spies
* São objetos falsos utilizados quando queremos manipular algum retorno que não faça parte do teste em si.

* São utilizados para isolar somente o bloco de codigo que estamos testando.

* Somente poder ser criado dentro do bloco 'describle' e 'it'.

* São removidos ao termino da execução da suite.

---
> # spyOn
* Serve para criar um mock (objeto fake) a ser utilizado nos testes.
* Um objeto spy contem varios atributos...
* Recebe como parametros o nome do objeto e metodo a serem utilizados como mock.

```javascript
describle("Suite de teste", function(){
  var calculadora = {
    somar: function(n1, n2){
      return n1 + n2;
    }
  };

  beforeAll(function() {
    spyOn(calculadora, "somar");
  });

  it("deve possuir um metodo somar como não definido", function() {
    expect(calculadora.somar(1, 1)).toBeUndefined();
  });

})
```
---
> toHaveBeenCalled
* serve para informar se um metodo do spy foi executado ao menos uma vez.
* Não possui parametros.

```javascript
describle("Suite de teste", function(){
  var calculadora = {
    somar: function(n1, n2){
      return n1 + n2;
    }
  };

  beforeAll(function() {
    spyOn(calculadora, "somar");
  });

  it("deve chamar o metodo somar ao menos uma vez", function() {
    calculadora.somar(1, 1);
    expect(calculadora.somar).toHaveBeenCalled();
  });

})
```
> toHaveBeenCalledTimes

> toHaveBeenCalledWith

> and.callThrough

> and.returnValue

> and.returnValues

> and.callFake

> and.throwError

> calls.any

> calls.count

> calls.argsFor

> calls.allArgs

> calls.all

> calls.mostRecent

> calls.first

> calls.reset

> createSpy

> createSpyOb


f
x - 
