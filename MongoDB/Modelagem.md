# 1. Modelagem

## Decida quais dados devem ser embutidos (embedded) e quais devem ser referenciados.

* Cliente

* Entregador

* Pedido

* Produto

***

**Respostas:**

**Cliente:** O `Cliente` deve ser referenciado, pois os dados do `Cliente` como: Nome, Endereço, Telefone, podem ser usados em vários pedidos.

**Entregador:** O `Entregador` deve ser referenciado, pois assim como os clientes, os entregadores podem ser associados a vários pedidos.

**Pedido:**  O `Pedido` deve ser embutido, pois o `Pedido` centraliza as informações do cliente, entregador e produtos.

**Produto:** O `Produto` deve ser referenciado, pois o `Produto` pode ser usado em vários pedidos diferentes.


## Escreva as seguintes consultas:

**a) Retorne todos os pedidos entregues por "Carlos Mendes".**

* Resposta: Para retornar os pedidos entregues por "Carlos Mendes", basta usar o seguinte comando:
```bash
db.pedidos.find({status: {$eq: "entregue"}})
```

***

**b) Liste os nomes dos produtos e os totais de vendas (quantidade vendida) agrupados.**

* Resposta: Para listar o nome do produto e o total de vendas agrupados, basta usar o seguinte comando:

```bash
db.pedidos.aggregate([
  { $unwind: "$produtos" },
  { $group: {
      _id: "$produtos.nome",
      quantidade_vendida: { $sum: "$produtos.quantidade" }
    }
  }
])
```

`obs:` Como não está especificado no código a quantidade vendida, irá mostrar 0.

***

**c) Encontre pedidos entregues perto da coordenada [-46.634, -23.551] com raio de 1 km.**

* Resposta: Para encontrar os pedidos entregues perto da coordenada especificada, basta usar o seguinte comando:

```bash
db.pedidos.find({
  status: "entregue",  
  local: {
    $near: {$geometry: { type: "Point", coordinates: [-46.634, -23.551] }, $maxDistance: 1000}}})
```

***

**d) Retorne todos os pedidos que contenham o item "Pizza Calabresa".**

* Resposta: Para retornar os pedidos que têm o item `Pizza Calabresa`, basta usar o seguinte comando:

```bash
db.pedidos.find({ "produtos.nome": "Pizza Calabresa" }) 
```

***

**e) Projete apenas o nome do cliente e os nomes dos produtos do pedido.**

* Resposta: Para projetar o nome do cliente e o nome dos produtos do pedido basta usar o seguinte comando:

```bash
db.pedidos.aggregate([
  { $unwind: "$produtos" },  
  { $group: { _id: "$nome", produtos: { $push: "$produtos.nome" }}}])
```