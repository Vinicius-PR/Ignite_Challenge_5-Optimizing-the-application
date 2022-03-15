# Ignite Challenge 5 - Otimizando a aplicação

Esse é um projeto simples onde eu tive que alterar a aplicação para ficar mais otimizada, mais performática. O que eu deveria usar são ferrametas do react como: 

* memo.

* useMemo (não foi usado nesse projeto).

* useCallback.

* Virtualização (não foi usado nesse projeto).

# Arquivos que eu deveria editar

* src/App.tsx
* src/components/Content.tsx
* src/components/SideBar.tsx

# Explicação da resolução

Inicialmente o `SideBar` estava renderizando por causa da propriedade "buttonClickCallback". Para resolver isso, bastou usar o **useCallback** no `App.tsx` na função "handleClickButton". Assim evita de ela sempre ser criada quando o App for renderizado, passando por cima do igualdade rasa. De acordo com a igualdade rasa, duas funções exatamente iguais em dois locais de memórias diferentes, irá constar como duas funções diferentes. O **useCallback** evita da função de ser criada do zero quando renderiza. Não há necessidade nesse caso pois a função sempre será a mesma. Assim, a função não muda de local na memoria e a propriedade "buttonClickCallback" não muda, evitando renderização extra.

Logo Apos isso. Vi que o `SideBar` ainda renderizava nos commits mas por outro motivo. Não era por causa das propriedade, mas sim por causa do componente pai. Sempre que o App renderizava, o `SideBar` também renderizava. Um comportamento normal do React. Se o componente pai renderizar, o filho também passa pelo processo do react de renderização. Já que o componente `SideBar` renderizara com as mesmas propriedades, nada melhor que usar o **memo**. Ele verifica se algum estado ou propriedade do componente mudou. Se não, o processo de renderização não inicia.

No componente `Content` tinha algumas mudanças para fazer. Durante os commits, o componente `Content` estava renderizando usando as mesmas propriedades movies e também por causa do App. Para contornar isso, o **memo** também foi usado aqui. Porém, uma função precisou ser passara para o **memo** já que a igualdade rasa para objetos não é uma opção. Foi usado o método próprio do JavaScript "Objest.is()".

# Poderia melhorar ainda mais?

Sim.
A aplicação faz uma requisição na API quando o `selectedGenreId`muda, retornando uma nova lista de movies. Do jeito que a aplicação está agora, não há problema nisso. há poucos filmes para ser listado.

Porém, numa aplicação do mundo real dificilmente será tão pouca informação assim. Caso retornasse um objeto com 500 ou até 1000 filmes do gênero selecionado, ficaria bem lenta a aplicação e a lista renderizada seria gigante.

Para contornar esse problema, o react-query pode ser usado, fazendo o uso do hook useQuerry e colocando um staleTime alto. Já que é bem improvável que as informações de um filme mude de um commit para o outro. Além disso, usar paginação. Evitando um scroll grande desnecessário.

# Conclusion

Otimização é um tema que não pode ser negligenciado, principalmente em aplicações médias ou grande. O ganho de performance aumenta e a experiência do usuário será bem melhor (e isso faz muita diferença).

Ignite bootcamp - React/Next course. Teacher: Diego Fernandes.