# T2_ORM_DJANGO
#Realización de operaciones con modelos usando el ORM DE DJANGO
#Proyecto creado en Entorno Virtualcon DJANGO 3.0
#Creado por: Johnny Duran C.
*********************************************************************
Para acceder al /admin con superusuario:
Usuario: johnny
Contraseña: johnny

*********************************************************************
#Insercion de codigos query en el ORM de Django
(InteractiveConsole)
>>> from apporm.models import Producto,Cliente,Factura,DetalleFactura 
>>> prod = Producto.objects.all() 
>>> prod
<QuerySet [<Producto: Quintal de Arroz>]>
>>> prod= Producto(descripcion='Atun Real',precio=2.50,stock=1500)      
>>> prod.save()
>>> prod = Producto.objects.all()
>>> prod
<QuerySet [<Producto: Atun Real>, <Producto: Quintal de Arroz>]>
>>> Producto.objects.create(descripcion='Leche la Vaquita',precio=1.25,stock=5000)  
<Producto: Leche la Vaquita>
>>> Producto.objects.create(descripcion='Mostaza',precio=1.80,stock=1200) 
<Producto: Mostaza>
>>> prod= Producto(descripcion='Tomate',precio=0.20,stock=600) 
>>> prod.save()
>>> prod= Producto(descripcion='Yogurt',precio=1.30,stock=100) 
>>> prod.save()
>>> prod= Producto(descripcion='Queso',precio=2.95,stock=1600)      
>>> prod.save()
>>> Producto.objects.create(descripcion='Cerelac',precio=1.90,stock=1100)        
<Producto: Cerelac>

#Inserte dos registros de clientes, 2 registros en el modelo factura con sus dos registros en el modelo detalle Factura con el ejemplo 1 y 2 respectivamente
#Dos Registros de clientes

>>> prod= Producto.objects.get(descripcion='Mostaza')   
>>> prod.descripcion
'Mostaza'
>>> cli=Cliente.objects.create(ruc='0963256895',nombre='Maria Duran',direccion='Milagro')
>>> cli.save()
>>> cli.producto.add(prod)
>>> cli=Cliente.objects.all().values('id','nombre','producto')
>>> cli
<QuerySet [{'id': 1, 'nombre': 'Johnny Duran', 'producto': 1}, {'id': 2, 'nombre': 'Maria Duran', 'producto': 4}]>
>>> cli=Cliente.objects.create(ruc='0952148532',nombre='Alexandra Castillo',direccion='Babahoyo')
>>> cli.save()
>>> cli.producto.add(prod)
>>> cli=Cliente.objects.all().values('id','nombre','producto')
>>> cli
<QuerySet [{'id': 1, 'nombre': 'Johnny Duran', 'producto': 1}, {'id': 2, 'nombre': 'Maria Duran', 'producto': 4}, {'id': 3, 'nombre': 'Alexandra Castillo', 'producto': 4}]>

#Insertar 2 registro en el modelo factura
>>> cli= Cliente.objects.get(nombre='Maria Duran')
>>> cli
<Cliente: Maria Duran>
>>> fac= Factura(cliente=cli,fecha='2020-08-04',total=10)
>>> fac.save()
>>> fac= Factura.objects.all().values('id','cliente__nombre')
>>> fac
<QuerySet [{'id': 2, 'cliente__nombre': 'Johnny Duran'}, {'id': 3, 'cliente__nombre': 'Maria Duran'}]>
>>> cli=Cliente.objects.all().values('id','nombre','direccion')
>>> cli
<QuerySet [{'id': 1, 'nombre': 'Johnny Duran', 'direccion': 'Milagro'}, {'id': 2, 'nombre': 'Maria Duran', 'direccion': 'Milagro'}, {'id': 3, 'nombre': 'Alexandra Castillo', 'direccion': 'Babahoyo'}]>
>>> cli=Cliente.objects.get(id=2)
>>> fac= Factura.objects.create(cliente=cli,fecha='2020-08-04',total=200)  
>>> fac= Factura.objects.all().values('id','cliente__nombre')
>>> fac
<QuerySet [{'id': 2, 'cliente__nombre': 'Johnny Duran'}, {'id': 3, 'cliente__nombre': 'Maria Duran'}, {'id': 4, 'cliente__nombre': 'Maria Duran'}]>

#Insertar 2 registros en el detalle de factura
>>> fac= Factura.objects.get(cliente__nombre='Johnny Duran')
>>> fac
<Factura: Johnny Duran>
>>> fac.fecha
datetime.date(2020, 8, 4)
>>> prod = Producto.objects.get(descripcion='Leche la Vaquita')
>>> prod
<Producto: Leche la Vaquita>
>>> DetalleFactura.objects.create(factura=fac,producto=prod,cantidad=10,precio=1.25,subtotal=5*1.25)
<DetalleFactura: DetalleFactura object (1)>
>>> prod= Producto.objects.get(descripcion='Cerelac')
>>> DetalleFactura.objects.create(factura=fac,producto=prod,cantidad=20,precio=1.90,subtotal=20*1.90) 
<DetalleFactura: DetalleFactura object (2)>
>>> fac= Factura.objects.get(cliente__nombre='Johnny Duran')       
>>> prod = Producto.objects.get(descripcion='Atun Real')   
>>> det= DetalleFactura(factura=fac,producto=prod,cantidad=2,precio=1.25,subtotal=2*1.25)
>>> det.save()
>>> det=DetalleFactura.objects.all().values('factura__id','factura__cliente__nombre','producto__descripcion','cantidad','precio')
>>> det
<QuerySet [{'factura__id': 2, 'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Leche la Vaquita', 'cantidad': 10.0, 'precio': 1.25}, {'factura__id': 2, 'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Cerelac', 'cantidad': 20.0, 'precio': 1.9}, {'factura__id': 2, 
'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Atun Real', 'cantidad': 2.0, 'precio': 1.25}]>

#Actualizar registros en los modelos
>>> prod= Producto.objects.get(descripcion='Tomate')
>>> prod.precio
0.2
>>> prod.precio= 0.50
>>> prod.save() 
>>> prod.descripcion
'Tomate'
>>> prod.precio
0.5
>>> Producto.objects.filter(descripcion='Tomate').update(precio=0.35)
1

#Modificar 2 registros de producto, cliente, factura y detalleFactura con el ejemplo 1 y 2 respectivamente
#Modificar 2 registro de producto
>>> prod= Producto.objects.get(id=6) 
>>> prod.descripcion
'Yogurt'
>>> prod.precio
1.3
>>> prod.precio= 1.50
>>> prod.save()
>>> prod.descripcion
'Yogurt'
>>> prod.precio
1.5
>>> Producto.objects.filter(descripcion='Yogurt').update(precio=1.60)
1

#Modificar 2 registros de cliente
>>> cli= Cliente.objects.get(id=3)
>>> cli.nombre
'Alexandra Castillo'
>>> cli.direccion
'Babahoyo'
>>> cli.direccion= 'Yaguachi'
>>> cli.save()
>>> cli.nombre
'Alexandra Castillo'
>>> cli.direccion
'Yaguachi'
>>> Cliente.objects.filter(nombre='Alexandra Castillo').update(direccion= 'Samborondon')
1

#Modificar 2 registros de factura
>>> cli=Cliente.objects.get(id=3)
>>> cli.nombre
'Alexandra Castillo'
>>> Factura.objects.filter(cliente__nombre='Maria Duran').update(cliente=cli)  
2
>>> Factura.objects.all().values('id','cliente__nombre','fecha')  
<QuerySet [{'id': 2, 'cliente__nombre': 'Johnny Duran', 'fecha': datetime.date(2020, 8, 4)}, {'id': 3, 'cliente__nombre': 'Alexandra Castillo', 'fecha': 
datetime.date(2020, 8, 4)}, {'id': 4, 'cliente__nombre': 'Alexandra Castillo', 'fecha': datetime.date(2020, 8, 4)}]>
>>> cli= Cliente.objects.get(nombre=’Johnny Duran')
fac= Factura.objects.get(cliente=cli)
<Factura: Johnny Duran>
>>> cli= Cliente.objects.get(nombre='Johnny Duran') 
>>> cli.id
1
>>> fac.cliente=cli
>>> fac.save()
>>> Factura.objects.all().values('id','cliente__nombre','fecha')
<QuerySet [{'id': 2, 'cliente__nombre': 'Johnny Duran', 'fecha': datetime.date(2020, 8, 4)}, {'id': 3, 'cliente__nombre': 'Alexandra Castillo', 'fecha': 
datetime.date(2020, 8, 4)}, {'id': 4, 'cliente__nombre': 'Alexandra Castillo', 'fecha': datetime.date(2020, 8, 4)}]>

#Modificar 2 registro de DetalleFactura
>>> det =DetalleFactura.objects.all().values('factura__id','factura__cliente__nombre','producto__descripcion','cantidad','precio') 
>>> det
<QuerySet [{'factura__id': 2, 'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Leche la Vaquita', 'cantidad': 10.0, 'precio': 1.25}, {'factura__id': 2, 'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Cerelac', 'cantidad': 20.0, 'precio': 1.9}, {'factura__id': 2, 
'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Atun Real', 'cantidad': 2.0, 'precio': 1.25}, {'factura__id': 6, 'factura__cliente__nombre': 'Franco Peralta', 'producto__descripcion': 'Tomate', 'cantidad': 5.0, 'precio': 0.35}, {'factura__id': 5, 'factura__cliente__nombre': 'Marcos Rendon', 'producto__descripcion': 'Quintal de Arroz', 'cantidad': 2.0, 'precio': 35.0}]>
>>> prod = Producto.objects.get(descripcion='Mostaza')  
>>> fac = Factura.objects.get(id=5)
>>>DetalleFactura.objects.filter(producto__descripcion='Quintal de Arroz').update(producto=prod)
1
>>> det =DetalleFactura.objects.all().values('factura__id','factura__cliente__nombre','producto__descripcion','cantidad','precio')
>>> det
<QuerySet [{'factura__id': 2, 'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Leche la Vaquita', 'cantidad': 10.0, 'precio': 1.25}, {'factura__id': 2, 'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Cerelac', 'cantidad': 20.0, 'precio': 1.9}, {'factura__id': 2, 
'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Atun Real', 'cantidad': 2.0, 'precio': 1.25}, {'factura__id': 6, 'factura__cliente__nombre': 'Franco Peralta', 'producto__descripcion': 'Tomate', 'cantidad': 5.0, 'precio': 0.35}, {'factura__id': 5, 'factura__cliente__nombre': 'Marcos Rendon', 'producto__descripcion': 'Mostaza', 'cantidad': 2.0, 'precio': 35.0}]>
>>> prod = Producto.objects.get(descripcion='Cerelac')
>>> fac = Factura.objects.get(id=6)
>>> det = DetalleFactura.objects.get(factura=fac)  
>>> det.producto = prod
>>> det.save()
>>> det=DetalleFactura.objects.all().values('factura__id','factura__cliente__nombre','producto__descripcion','cantidad','precio')
>>> det
<QuerySet [{'factura__id': 2, 'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Leche la Vaquita', 'cantidad': 10.0, 'precio': 1.25}, {'factura__id': 2, 'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Cerelac', 'cantidad': 20.0, 'precio': 1.9}, {'factura__id': 2, 
'factura__cliente__nombre': 'Johnny Duran', 'producto__descripcion': 'Atun Real', 'cantidad': 2.0, 'precio': 1.25}, {'factura__id': 6, 'factura__cliente__nombre': 'Franco Peralta', 'producto__descripcion': 'Cerelac', 'cantidad': 5.0, 'precio': 0.35}, {'factura__id': 5, 'factura__cliente__nombre': 'Marcos 
Rendon', 'producto__descripcion': 'Mostaza', 'cantidad': 2.0, 'precio': 35.0}]>

#Eliminar registros en los modelos
>>> prod = Producto.objects.get(id=7) 
>>> prod.descripcion
'Queso'
>>> prod.delete()
(1, {'apporm.Producto': 1})
>>> Producto.objects.filter(descripcion='pimiento').delete() 
(1, {'apporm.Producto': 1})

#Querys en un modelo
>>> prod=Producto.objects.all() 
>>> prod
<QuerySet [<Producto: cebolla>, <Producto: Cerelac>, <Producto: Yogurt>, <Producto: Tomate>, <Producto: Mostaza>, <Producto: Leche la Vaquita>, <Producto: Atun Real>, <Producto: Quintal de Arroz>]>
>>> prod=Producto.objects.get(id=5)
>>> prod
<Producto: Tomate>
>>> Producto.objects.filter(id__lte=5)
<QuerySet [<Producto: Tomate>, <Producto: Mostaza>, <Producto: Leche la Vaquita>, <Producto: Atun Real>, <Producto: Quintal de Arroz>]>
>>> Producto.objects.exclude(descripcion__icontains='Quintal')  
<QuerySet [<Producto: cebolla>, <Producto: Cerelac>, <Producto: Yogurt>, <Producto: Tomate>, <Producto: Mostaza>, <Producto: Leche la Vaquita>, <Producto: Atun Real>]>
>>> Producto.objects.filter(id__gte=2) 
<QuerySet [<Producto: cebolla>, <Producto: Cerelac>, <Producto: Yogurt>, <Producto: Tomate>, <Producto: Mostaza>, <Producto: Leche la Vaquita>, <Producto: Atun Real>]>
>>> Producto.objects.filter(id__gt=2).values('id','descripcion') 
<QuerySet [{'id': 9, 'descripcion': 'cebolla'}, {'id': 8, 'descripcion': 'Cerelac'}, {'id': 6, 'descripcion': 'Yogurt'}, {'id': 5, 'descripcion': 'Tomate'}, {'id': 4, 'descripcion': 'Mostaza'}, {'id': 3, 'descripcion': 'Leche la Vaquita'}]>
>>> Producto.objects.filter(id__lt=2).values('id','descripcion') 
<QuerySet [{'id': 1, 'descripcion': 'Quintal de Arroz'}]>
>>> Producto.objects.filter(descripcion='cebolla').values('id','descripcion')                                                                            
<QuerySet [{'id': 9, 'descripcion': 'cebolla'}]>

#Querys de varios modelos (relacionados)
>>> Factura.objects.filter(cliente__nombre='Franco Peralta')
<QuerySet [<Factura: Franco Peralta>]>
>>> cli.factura_set.all()
<QuerySet [<Factura: Johnny Duran>]>
>>> Factura.objects.select_related('cliente').filter(cliente__nombre='Franco Peralta')
<QuerySet [<Factura: Franco Peralta>]>
>>> Cliente.objects.prefetch_related('producto').filter(nombre='Franco Peralta').values('nombre','producto__descripcion')
<QuerySet [{'nombre': 'Franco Peralta', 'producto__descripcion': 'Cerelac'}]>
