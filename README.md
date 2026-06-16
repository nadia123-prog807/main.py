# main.py
"""
MÓDULO RESTAURANTE
Autor: [Tu Nombre Completo]
Descripción: Contiene la lógica principal del sistema de gestión del restaurante.
Coordina los productos, clientes y las operaciones del negocio.
"""

from modelos.producto import Producto
from modelos.cliente import Cliente


class Restaurante:
    """
    Clase que gestiona las operaciones del restaurante.
    """
    
    def __init__(self, nombre):
        """
        Constructor de la clase Restaurante.
        
        Parámetros:
        nombre (str): Nombre del restaurante
        """
        self.nombre = nombre
        self.productos = []      # Lista de objetos Producto
        self.clientes = []       # Lista de objetos Cliente
        self.contador_productos = 0
        self.contador_clientes = 0
    
    # ========== MÉTODOS PARA PRODUCTOS ==========
    
    def agregar_producto(self, nombre, precio, categoria):
        """
        Crea y agrega un nuevo producto al menú.
        
        Parámetros:
        nombre (str): Nombre del producto
        precio (float): Precio del producto
        categoria (str): Categoría del producto
        """
        self.contador_productos += 1
        producto = Producto(self.contador_productos, nombre, precio, categoria)
        self.productos.append(producto)
        print(f"✅ Producto '{nombre}' agregado al menú (ID: {self.contador_productos})")
        return producto
    
    def listar_productos(self, solo_disponibles=False):
        """
        Muestra todos los productos del menú.
        
        Parámetros:
        solo_disponibles (bool): Si es True, muestra solo los productos disponibles
        """
        print("\n" + "=" * 50)
        print(f"   MENÚ DE {self.nombre.upper()}")
        print("=" * 50)
        
        productos_filtrados = self.productos
        if solo_disponibles:
            productos_filtrados = [p for p in self.productos if p.disponible]
        
        if not productos_filtrados:
            print("   No hay productos disponibles.")
        else:
            for p in productos_filtrados:
                p.mostrar_info()
                print()
    
    def buscar_producto(self, id_producto):
        """
        Busca un producto por su ID.
        
        Parámetros:
        id_producto (int): ID del producto a buscar
        
        Retorna:
        Producto: El producto encontrado o None si no existe
        """
        for producto in self.productos:
            if producto.id_producto == id_producto:
                return producto
        return None
    
    # ========== MÉTODOS PARA CLIENTES ==========
    
    def registrar_cliente(self, nombre, telefono, email=""):
        """
        Registra un nuevo cliente en el sistema.
        
        Parámetros:
        nombre (str): Nombre del cliente
        telefono (str): Teléfono del cliente
        email (str): Email del cliente (opcional)
        """
        self.contador_clientes += 1
        cliente = Cliente(self.contador_clientes, nombre, telefono, email)
        self.clientes.append(cliente)
        print(f"✅ Cliente '{nombre}' registrado (ID: {self.contador_clientes})")
        return cliente
    
    def listar_clientes(self):
        """
        Muestra todos los clientes registrados.
        """
        print("\n" + "=" * 50)
        print("   LISTA DE CLIENTES")
        print("=" * 50)
        
        if not self.clientes:
            print("   No hay clientes registrados.")
        else:
            for c in self.clientes:
                c.mostrar_info()
                print()
    
    def buscar_cliente(self, id_cliente):
        """
        Busca un cliente por su ID.
        
        Parámetros:
        id_cliente (int): ID del cliente a buscar
        
        Retorna:
        Cliente: El cliente encontrado o None si no existe
        """
        for cliente in self.clientes:
            if cliente.id_cliente == id_cliente:
                return cliente
        return None
    
    # ========== OPERACIONES DEL RESTAURANTE ==========
    
    def realizar_pedido(self, id_cliente, id_producto):
        """
        Realiza un pedido asociando un cliente con un producto.
        
        Parámetros:
        id_cliente (int): ID del cliente
        id_producto (int): ID del producto
        """
        # Buscar cliente
        cliente = self.buscar_cliente(id_cliente)
        if not cliente:
            print(f"❌ Cliente con ID {id_cliente} no encontrado.")
            return
        
        # Buscar producto
        producto = self.buscar_producto(id_producto)
        if not producto:
            print(f"❌ Producto con ID {id_producto} no encontrado.")
            return
        
        # Verificar disponibilidad
        if not producto.disponible:
            print(f"❌ El producto '{producto.nombre}' no está disponible.")
            return
        
        # Registrar el pedido
        cliente.agregar_compra(producto)
        print(f"🍽️ Pedido realizado: {cliente.nombre} pidió '{producto.nombre}'")
    
    def mostrar_resumen(self):
        """
        Muestra un resumen del estado del restaurante.
        """
        print("\n" + "=" * 50)
        print(f"   RESUMEN DE {self.nombre.upper()}")
        print("=" * 50)
        print(f"   📦 Total de productos: {len(self.productos)}")
        print(f"   👤 Total de clientes: {len(self.clientes)}")
        
        productos_disponibles = len([p for p in self.productos if p.disponible])
        print(f"   ✅ Productos disponibles: {productos_disponibles}")
        
        total_ventas = sum(len(c.historial_compras) for c in self.clientes)
        print(f"   🛒 Total de pedidos realizados: {total_ventas}")
        print("=" * 50)
