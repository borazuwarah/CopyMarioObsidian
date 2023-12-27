Por ejemplo vamos a crear una base de datos y luego creamos una colección llamada vídeos:
![[Pasted image 20230602095149.png]]
Y aquí creamos otra colección y ya tenemos 2:
![[Pasted image 20230602095219.png]]
### COMO ELIMINAR UNA COLECCIÓN
Para eliminar una colección, usamos db.suscriptores.drop()
```sql
db.suscriptores.drop()
```
Y ahora vemos que ya no existe la colección suscriptores:
![[Pasted image 20230602095428.png]]
# LOS DOCUMENTOS
Los documentos son la forma de enviar y guardar los datos en mongoDB, los cuales van en formato json. Por ejemplo vamos a almacenar estos datos:
```sql
{
  "nombre": "Juan Pérez",
  "edad": 30,
  "ciudad": "Ciudad de México",
  "intereses": ["programación", "viajes", "fotografía"],
  "educacion": {
    "titulo": "Ingeniero en Sistemas",
    "universidad": "Universidad Nacional Autónoma de México"
  }
}
```
Podemos guardar esta data en json de la siguiente forma:
![[Pasted image 20230602095946.png]]
### CONSULTAR DATOS DE UNA COLECCIÓN
Para ver los datos que hemos añadido anteriormente en la colección vídeos, lo hacemos de esta forma:
![[Pasted image 20230602100110.png]]
También podemos insertar un dato aislado de la siguiente forma:
![[Pasted image 20230602100703.png]]
También podemos insertar un dato que tenga más de un atributo:
![[Pasted image 20230602100914.png]]
Lo interesante es que la estructura de esta información es diferente, no tiene estructura tal y como vemos, ya que podemos definir la misma estructura desde el lenguaje de programación si así lo preferimos:
![[Pasted image 20230602101008.png]]
### ELIMINAR UN DATO DE UNA COLECCIÓN
Para eliminar un solo dato de una colección, lo hacemos de la siguiente forma:
```sql
db.videos.deleteMany({ nombre: 'Juan Pérez' })
```
![[Pasted image 20230602101940.png]]
Y ahora toda la data de este nombre se habrá eliminado:
![[Pasted image 20230602102039.png]]
# FILTRAR UN DATO EN CONCRETO
También podemos obtener únicamente todos los datos de un titulo en específico, por ejemplo del campo nombre o precio:
![[Pasted image 20230606091748.png]]
![[Pasted image 20230606091834.png]]
![[Pasted image 20230606092200.png]]