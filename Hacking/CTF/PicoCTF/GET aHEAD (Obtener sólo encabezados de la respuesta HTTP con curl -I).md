Tenemos esta web:
![[Pasted image 20230307081841.png]]
Donde si hacemos un curl nos proporciona su contenido HTML:
![[Pasted image 20230307081911.png]]
Pero podemos utilizar el parámetro -I de curl para obtener sólo los encabezados de la respuesta HTTP para una URL determinada. En otras palabras, el parámetro -I solicita solo la información de encabezado de una respuesta HTTP y no el cuerpo de la respuesta; y aquí obtenemos la flag:
![[Pasted image 20230307081957.png]]

