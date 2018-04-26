## Nanoc tips

### Refs

- http://rubylearning.com/satishtalim/ruby_blocks.html
- http://www.rubydoc.info/github/nanoc/nanoc/Nanoc/Int/CompilerDSL#compile-instance_method
-
Tenho que compilar para atualizar:

```bash
$ nanoc compile
$ nanoc view
```

Tem a seção *frontmatter* do arquivo, onde eu posso colocar metainformações usando o padrão yaml, ou em um arquivo *.yaml* com o mesmo nome da página.

```html
# content/index.html
---
title: An amazing title
foo: bar
---

# layouts/default.html
<title>A Brand New Nanoc Site - <%= @item[:title] %></title>
    ...
<h2>Documentation with <%= item[:foo]%></h2>
```

Definição do yield: `The <%= yield %> instruction is replaced with the item’s compiled content when compiling.`

Definição de filtros: `Nanoc has filters, which transform content from one format into another.`

Posso definir uma route ou direito no compile, para cada arquivo:

```ruby
# Assim
compile '/**/*.html' do
  layout '/default.*'
  write item.identifier.without_ext + '/index.html'
end

compile '/**/*.md' do
 filter :kramdown
 layout '/default.*'
 write item.identifier.without_ext + '/index.html'
end

# Ou assim
route '/**/*.{html,md,css}' do
  if item.identifier =~ '/index.*'
    '/index.html'
  else
    item.identifier.without_ext + '/index.html'
  end
end
```

Posso definir funções dentro da pasta `lib` e chamar elas das views, isso ajuda a abstrair a lógica da view.
