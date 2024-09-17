
### Tickets

La tabla de tickets es la más importante para el sistema. Es donde están las ventas, que es el insumo principal para el forecasting.

Las relaciones con otras tablas son:

- los métodos de pago que se usaron en el ticket.
- Las promociones aplicadas en este ticket
- y los Skus que se compraron en este ticket.


![[Pasted image 20240916181404.png]]


`CREATE TABLE tickets (`
  `id int NOT NULL,`
  `code int NOT NULL,`
  `client_rut int NOT NULL,`
  `date date NOT NULL,`
  `subtotal double NOT NULL,`
  `iva double NOT NULL,`
  `total double NOT NULL,`
  `document_type text NOT NULL,`
  `accumulate_points double NOT NULL,`
  `consumed_points double NOT NULL,`
  `currency_id int NOT NULL,`
  `channel_id int NOT NULL,`
  `sale_point_id int NOT NULL,`
  `fidelity text CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci,`
  `organization_id int NOT NULL,`
  `created_at timestamp NULL DEFAULT CURRENT_TIMESTAMP,`
  `updated_at datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`
`) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;`


### Tickets_skus

Contiene qué skus se vendieron en cada ticket, a qué precio y qué cantidad. 
**tickets.id = tickets_skus.ticket_id**
Validar si la suma de unit_price * quantity de todos los skus de un ticket_id equivale a "total" del ticket. Creo que a subtotal, sacando el iva.

![[Pasted image 20240916181728.png]]


### Skus

La tabla de SKUs es de las más importantes del sistema, se usa en todos los módulos porque tanto las compras y reposiciones como los precios, promociones y acciones de fidelidad se basan en los SKUs.

Se relaciona con los artículos (un artículo puede tener varios SKUs), con las sucursales en que se vende, con los depósitos en que se almacena, con las órdenes de compra y reposición en que se incluyen, con las promociones en que se incluyen, y los cambios de precio que los afectan.

![[Pasted image 20240916182540.png]]
![[Pasted image 20240916182607.png]]

SKU es una variedad dentro de un mismo articulo, puede cambiar el talle por ejemplo pero el precio tiene sentido que sea el mismo y que por eso el precio venga por articulo.


**Pregunta:** no es estática, se va actualizando con las ordenes de compra y reposicion, storage unit, etc (puede haber mas de un storage unit?), total stock, etc.

### Artículos

Registramos los artículos, que el cliente debe importar de alguna forma al sistema (puede ser API, Excel o carga manual), para luego poder referenciarlos en la definición del pricing de cada uno, crear promociones por artículo, definir compra y reposición de los mismos, y procesar las ventas para sugerir acciones.

Artículos (junto con SKUs, clientes y tickets) es una de las tablas clave del sistema.

Luego están las relaciones del artículo con otras tablas como:

- Artículos por unidad de negocio: la empresa puede tener varias unidades de negocio que trabajen los mismos, o diferentes artículos.
- Precio de los artículos: esta tabla es el centro del módulo de Pricing. Es donde se registran los precios asignados a los artículos.
- Proveedores de los artículos: esta tabla es muy importante para el módulo de Compras, porque se usa para definir a quién comprarle un artículo. De acá tomamos quienes son los proveedores que ofrecen cada artículo.


SKU es una variedad dentro de un mismo articulo, puede cambiar el talle por ejemplo pero el precio tiene sentido que sea el mismo y que por eso el precio venga por articulo.

![[Pasted image 20240916184012.png]]
![[Pasted image 20240916184026.png]]


### Categories

Las categorías van atadas a los artículos, permiten organizarlos en diferentes familias. Un artículo siempre pertenece a una categoría.

La categoría + el artículo + el SKU conforman el mercadológico de la unidad de negocio.

Una categoría puede tener otra categoría padre y subcategorías.

Usamos las categorías más que nada para análisis y recomendaciones. También como forma de agrupar artículos y de filtrar grandes cantidades de artículos de forma rápida.

Luego está la relación de las categorías con las unidades de negocio, que representa qué categorías tiene cada unidad de negocio.


**Un articulo tiene asociada una categoria de las de bajo nivel, hay que ir trackeando los "father" para encontrar todo el arbol.**

![[Pasted image 20240916185022.png]]