```python
from telethon import TelegramClient, events
import asyncio
import nest_asyncio

nest_asyncio.apply()

api_id = 'API' # Reemplaza TU_API_ID con el ID de API de tu aplicación de Telegram
api_hash = 'API HASH' # Reemplaza TU_API_HASH con el hash de API de tu aplicación de Telegram
phone_number = '+34NUMERO' # Reemplaza TU_NUMERO_DE_TELEFONO con tu número de teléfono (incluyendo el código de país)

async def main():
    # Crea el cliente de Telegram
    client = TelegramClient('session_name', api_id, api_hash)

    # Inicia sesión con el número de teléfono proporcionado
    await client.start(phone_number)

    # Espera a que se complete la autenticación
    await client.is_user_authorized()

    link = 'https://t.me/automatizacion' # Reemplaza nombre_del_grupo con el nombre del grupo de Telegram
    channel = link.split('/')[-1]

    # Usa el cliente para leer mensajes del chat de Telegram
    async for message in client.iter_messages(channel):
        print(message.sender_id, ':', message.text)

    # Cierra la conexión con el cliente
    await client.disconnect()

# Ejecuta la función principal
asyncio.run(main())

```
## HACER QUE A CADA CONSULTA SE BORREN LOS MENSAJES DEL GRUPO

```python
from telethon import TelegramClient, events
import asyncio
import nest_asyncio

nest_asyncio.apply()

api_id = 'API' 
api_hash = 'API HASH' 
phone_number = '+34NUMERO' 

async def funcion():
    # Crea el cliente de Telegram
    client = TelegramClient('session_name', api_id, api_hash)

    await client.start(phone_number)

    await client.is_user_authorized()

    link = 'https://t.me/automatizacion' 
    channel = link.split('/')[-1]

    # crear una lista vacía para almacenar los resultados
    resultados = []

    async for message in client.iter_messages(channel):
        resultado = (message.sender_id, ':', message.text)
        resultados.append(resultado)
        await client.delete_messages(channel, message) # Borramos los menssajes del grupo.

    await client.disconnect()

    # imprimir la lista de resultados
    return resultados

resultados = asyncio.run(funcion())
print(resultados)
```