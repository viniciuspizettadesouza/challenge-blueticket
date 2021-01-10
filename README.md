# challenge-blueticket
### blueticket - desafio técnico vue.js

Este desafio tem o objetivo de avaliar melhor suas skills como desenvolvedor, não apenas raciocínio lógico, mas também organização, capacidade de resolver
problemas, engenhosidade, clareza do código, detalhamento visual, etc.

O que esperamos é que seja desenvolvido um pequeno projeto contendo os elementos básicos, front-end e um pouco de infra!

#### Requisitos
● Utilizando a API de mapas da Google https://developers.google.com/maps/documentation/geocoding/intro buscar as coordenadas de um dado endereço fornecido pelo usuário.
● Salvar o histórico de buscas de endereços e utilizar o mesmo para autocompletar o campo de busca.
● Requisitar a localização (coordenadas) do usuário ao abrir a página ou de outra forma que você considere natural no aspecto da usabilidade.
● Com as coordenadas vindas da busca pela API do google ou pela localização do browser do usuário deve ser feita uma busca à Open Weather API
(https://openweathermap.org/api).
○ Com o resultado dessa busca devem ser apresentadas as informações básicas do clima atual das coordenadas, bem como a previsão dos próximos dias.
○ Os dados climáticos são atualizados de hora em hora, da mesma forma um mesmo endereço não muda de geolocalização. Tendo isso em vista deve ser criado um cache nas buscas, evitando a necessidade de consultas desnecessárias das APIs.

Detalhes
● Deve ser responsivo;
● Utilizar Vue.js como linguagem, podendo utilizar uma biblioteca ou framework a sua escolha;
● A aplicação deve ser hospedada em um serviço da nuvem (heroku, aws, azure, ou qualquer outra infraestrutura);
● Junto com o link do código (github, bitbucket, etc) deve ser enviada uma url do projeto rodando;
● O projeto deverá ser de fácil reprodução local e para tal deve conter um README explicando os passos necessários;

O que será avaliado
● Boas práticas, organização do código, arquitetura
● Bom uso da linguagem e ferramentas
● Uso do Git

Finalizado, nos conte quais as facilidades na execução do projeto e quais foram os principais desafios encontrados.

# blueticket_frontend

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
