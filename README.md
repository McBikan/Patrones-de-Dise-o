#Patrones de Diseño

Para nuestro proyecto se pidieron usar 3 patrones de diseño:

* Adapter
* Abstract Factory
* State
 
##Descripción del Proyecto
Desarrollaremos un sistema de pedidos de restaurante que permita a los clientes realizar pedidos, a los chefs preparar los alimentos y a los camareros servir los platos. Este sistema deberá gestionar la comunicación entre los diferentes componentes y garantizar que los pedidos se procesen de manera eficiente.
 

## Adapter
El patrón Adapter se utiliza para hacer que una clase existente sea compatible con una interfaz diferente. Esto se logra mediante la creación de una clase intermedia, el "Adapter", que traduce las solicitudes de la nueva interfaz en llamadas a la clase existente.

###Caso de Uso
En nuestro caso de uso el código a usar sería:
```ruby
# Interfaz común para gestionar pedidos
class GestorPedidos
  def registrarPedido(pedido)
  end

  def prepararPedido(pedido)
  end

  def servirPedido(pedido)
  end
end
# Clase para gestionar pedidos en línea
class PedidoEnLinea < GestorPedidos
  def registrarPedido(pedido)
    puts "Pedido en línea registrado: #{pedido}"
  end

  def prepararPedido(pedido)
    puts "Preparando pedido en línea: #{pedido}"
  end

  def servirPedido(pedido)
    puts "Sirviendo pedido en línea: #{pedido}"
  end
end

# Clase para gestionar pedidos en persona en el restaurante
class PedidoEnPersona < GestorPedidos
  def registrarPedido(pedido)
    puts "Pedido en persona registrado: #{pedido}"
  end

  def prepararPedido(pedido)
    puts "Preparando pedido en persona: #{pedido}"
  end

  def servirPedido(pedido)
    puts "Sirviendo pedido en persona: #{pedido}"
  end
end

# Adaptador para pedidos en línea
class AdaptadorPedidoEnLinea < GestorPedidos
  def initialize(pedido_en_linea)
    @pedido_en_linea = pedido_en_linea
  end

  def registrarPedido(pedido)
    @pedido_en_linea.registrarPedido(pedido)
  end

  def prepararPedido(pedido)
    @pedido_en_linea.prepararPedido(pedido)
  end

  def servirPedido(pedido)
    @pedido_en_linea.servirPedido(pedido)
  end
end
```

## State
El patrón Abstract Factory proporciona una forma de crear familias de objetos relacionados sin especificar sus clases concretas. Define una interfaz común para crear objetos y luego implementa varias fábricas concretas que producen objetos coherentes. Esto permite la creación de objetos sin acoplar el código a las clases específicas.
###Caso de Uso:
Vamos a definir una clase "EstadoPedido" que declare 3 métodos diferentes:
"confirmar","preparar" y servir.

```ruby
class EstadoPedido
  def confirmar(pedido)
  end

  def preparar(pedido)
  end

  def servir(pedido)
  end
end 
```

A continuación, crearemos clases concretas para cada estado del pedido, como EstadoConfirmado, EstadoEnPreparacion, EstadoListo y EstadoEntregado. Cada una de estas clases implementará los métodos de acuerdo con su comportamiento específico.

```ruby
class EstadoConfirmado < EstadoPedido
  def confirmar(pedido)
    # Implementación para el estado "Confirmado"
  end
end

class EstadoEnPreparacion < EstadoPedido
  def preparar(pedido)
    # Implementación para el estado "En Preparación"
  end
end

class EstadoListo < EstadoPedido
  def servir(pedido)
    # Implementación para el estado "Listo"
  end
end

class EstadoEntregado < EstadoPedido
  # No es necesario implementar métodos para "Entregado"
end
```
Luego, en la clase principal que maneja los pedidos, puedes utilizar una referencia al objeto de estado actual para delegar el comportamiento según el estado actual del pedido.

```ruby
class Pedido
  attr_accessor :estado_pedido

  def initialize
    @estado_pedido = EstadoConfirmado.new
  end

  def confirmar
    @estado_pedido.confirmar(self)
  end

  def preparar
    @estado_pedido.preparar(self)
  end

  def servir
    @estado_pedido.servir(self)
  end
end
```


## Abstract Factory
El patrón State se utiliza para modelar objetos que pueden cambiar su comportamiento cuando su estado interno cambia. En lugar de tener un objeto con un solo comportamiento monolítico, se crean clases separadas para cada estado posible y se delega el comportamiento a la clase de estado actual. Esto permite que un objeto altere su comportamiento de manera dinámica según su estado interno.

###Caso de Uso
Primero, definimos una interfaz abstracta MenuFactory que declara métodos para crear diferentes tipos de menús, platos principales y bebidas.

```ruby
class MenuFactory
  def crear_menu
  end

  def crear_plato_principal
  end

  def crear_bebida
  end
end
```
Luego, creamos dos fábricas concretas: MenuDesayunoFactory y MenuCenaFactory. Cada fábrica implementa los métodos para crear menús, platos principales y bebidas específicas para su tipo de menú.
```ruby
class MenuDesayunoFactory < MenuFactory
  def crear_menu
    return "Menú de Desayuno"
  end

  def crear_plato_principal
    return "Tostadas con huevos"
  end

  def crear_bebida
    return "Café"
  end
end

class MenuCenaFactory < MenuFactory
  def crear_menu
    return "Menú de Cena"
  end

  def crear_plato_principal
    return "Filete de res a la parrilla"
  end

  def crear_bebida
    return "Vino tinto"
  end
end
```

En la clase principal que maneja los pedidos, utilizamos una referencia a la fábrica de menús actual para crear menús, platos principales y bebidas según el tipo de menú deseado.

```ruby
class Pedido
  attr_accessor :menu_factory

  def initialize(menu_factory)
    @menu_factory = menu_factory
  end

  def crear_pedido
    menu = @menu_factory.crear_menu
    plato_principal = @menu_factory.crear_plato_principal
    bebida = @menu_factory.crear_bebida

    # Realizar acciones relacionadas con el pedido
    puts "Pedido: #{menu}, Plato Principal: #{plato_principal}, Bebida: #{bebida}"
  end
end
```
#Proyecto Completo
```ruby
# Patrón Adapter
class SistemaVenta
  def procesoPago
  end
end

class PagoEfectivo < SistemaVenta
  def procesoPago(cantidadPagar)
    puts "Ha pagado en efectivo la cantidad de $#{cantidadPagar}"
  end
end

class PagoOnlineAdapter < SistemaVenta
  def initialize(pagoOnline)
    @pagoOnline = pagoOnline
  end

  def procesoPago(cantidadPagar)
    @pagoOnline.hacerPago(cantidadPagar)
  end
end

class PagoOnline
  def hacerPago(cantidadPagar)
    puts "Ha pagado de manera online la cantidad de $#{cantidadPagar}"
  end
end

# Patrón State
class EstadoPedido
  def confirmar(pedido)
  end

  def preparar(pedido)
  end

  def servir(pedido)
  end
end

class EstadoConfirmado < EstadoPedido
  def confirmar(pedido)
    puts "Pedido confirmado."
  end
end

class EstadoEnPreparacion < EstadoPedido
  def preparar(pedido)
    puts "Pedido en preparación."
  end
end

class EstadoListo < EstadoPedido
  def servir(pedido)
    puts "Pedido listo para servir."
  end
end

class EstadoEntregado < EstadoPedido
  def servir(pedido)
    puts "Pedido entregado."
  end
end

class Pedido
  attr_accessor :estado_pedido

  def initialize
    @estado_pedido = EstadoConfirmado.new
  end

  def confirmar
    @estado_pedido.confirmar(self)
  end

  def preparar
    @estado_pedido.preparar(self)
  end

  def servir
    @estado_pedido.servir(self)
  end
end

# Patrón Abstract Factory
class MenuFactory
  def crear_menu
  end

  def crear_plato_principal
  end

  def crear_bebida
  end
end

class MenuDesayunoFactory < MenuFactory
  def crear_menu
    return "Menú de Desayuno"
  end

  def crear_plato_principal
    return "Tostadas con huevos"
  end

  def crear_bebida
    return "Café"
  end
end

class MenuCenaFactory < MenuFactory
  def crear_menu
    return "Menú de Cena"
  end

  def crear_plato_principal
    return "Filete de res a la parrilla"
  end

  def crear_bebida
    return "Vino tinto"
  end
end

class Pedido
  attr_accessor :menu_factory

  def initialize(menu_factory)
    @menu_factory = menu_factory
  end

  def crear_pedido
    menu = @menu_factory.crear_menu
    plato_principal = @menu_factory.crear_plato_principal
    bebida = @menu_factory.crear_bebida

    puts "Pedido: #{menu}, Plato Principal: #{plato_principal}, Bebida: #{bebida}"
  end
end

# Uso del patrón Adapter
puts "Ejemplo de Adapter:"
pago_efectivo = PagoEfectivo.new
pago_efectivo.procesoPago(100)

pago_online = PagoOnline.new
adaptador_online = PagoOnlineAdapter.new(pago_online)
adaptador_online.procesoPago(200)

# Uso del patrón State
puts "\nEjemplo de State:"
pedido = Pedido.new
pedido.confirmar
pedido.preparar
pedido.servir

# Uso del patrón Abstract Factory
puts "\nEjemplo de Abstract Factory:"
menu_desayuno_factory = MenuDesayunoFactory.new
pedido_desayuno = Pedido.new(menu_desayuno_factory)
pedido_desayuno.crear_pedido

menu_cena_factory = MenuCenaFactory.new
pedido_cena = Pedido.new(menu_cena_factory)
pedido_cena.crear_pedido
```
## Salida del Proyecto:

```ruby
Ejemplo de Adapter:
Ha pagado en efectivo la cantidad de $100
Ha pagado de manera online la cantidad de $200

Ejemplo de State:
Pedido confirmado.
Pedido en preparación.
Pedido listo para servir.

Ejemplo de Abstract Factory:
Pedido: Menú de Desayuno, Plato Principal: Tostadas con huevos, Bebida: Café
Pedido: Menú de Cena, Plato Principal: Filete de res a la parrilla, Bebida: Vino tinto
```
